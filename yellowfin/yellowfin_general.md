## Yellowfin General

Yellowfin can be setup once the application is deployed to production. Once yellowfin is setup, the  app can be re-deployed with the yellowfin envars.

- See the service ticket word document for the hostname in the PRODUCTION table under YELLOWFIN
- The login information (YF_USER & YF_PASSWORD) is found in pleasant under Reporting > Yellowfin > Top Level Adminstration

See Nnamdi's notes on setting up Yellowfin [here](https://github.com/CEXNIbe/ReadMe/wiki/v5-Yellowfin-Setup) and the walk-through pdf in documents.

#### Steps
- visit `https://report<number>.i-sight.com` to get started
- sign in with the credentials from pleasant
- for the most part, follow along with the pdf guide.
- take note of the fields that have `kind: 'editable'` [1]

#### [1] fields with kind: editable
- open the code for the app
- copy the case fields from the case/index.js file into a blank document and extract the names of the fields that have `kind: editable`

#### Connecting the database
On page 10 of the guide, which walks through connecting the database, these are the variables that need to be filled in:
- name: <client_name> Prod
- database type: postgres
- database host: from the Word document, database internal IP
- database port: leave is is (5432)
- database name: isight
- username: postgres
- password: postgres

#### Adding tables
- add `vw_sys_case`
- add all child docs tables that start with `vw_sys_` (ex: party, email, note, attachment, interview, etc...)

#### Table Options > Columns
- Choose "Select All" and then un-select columns as described in the walk-through document.

#### ID virtual tables
- In the database, a function called `getusernamefromid` needs to be created in order to connect IDs to names. Log into the data base and paste the following code to create the function in the database:
```
CREATE OR REPLACE FUNCTION public.getusernamefromid(user_id text) 
RETURNS text AS
  $BODY$Declare
  user_full_name text; 

BEGIN
SELECT vw_sys_user.name INTO user_full_name FROM vw_sys_user WHERE vw_sys_user.id=user_id; 
RETURN user_full_name;
END;$BODY$
LANGUAGE plpgsql VOLATILE
COST 100;
ALTER FUNCTION public.getusernamefromid(text)
OWNER TO postgres;
```

- Once done, paste the following code that will return all the fields that needs to use the function in yellowfin:
```
SELECT
  CONCAT('vw_',table_name) as table_name,
  column_name,
  CONCAT('getusernamefromid(','vw_',table_name,'.',column_name,') as ',column_name) as yellowfin_field
FROM
  information_schema.columns
WHERE
  data_type IN ('character varying')
  AND character_maximum_length = '36' 
  AND LEFT(table_name,3)='sys'
  AND table_name NOT IN
    ('sys_assignment_rule', 'sys_comment', 'sys_dialing_instruction', 'sys_document', 'sys_emailrule', 'sys_employee', 'sys_escalation', 'sys_event', 'sys_grid_column', 'sys_grid_filter',
    'sys_holiday_entry', 'sys_link_match', 'sys_link_snapshot', 'sys_link', 'sys_list', 'sys_listitem', 'sys_log', 'sys_message', 'sys_notification', 'sys_passwordhistory',
    'sys_permission', 'sys_person', 'sys_response', 'sys_role_filter', 'sys_role_permission', 'sys_searches', 'sys_session', 'sys_template', 'sys_todo_automation',
    'sys_translation', 'sys_trigger', 'sys_user_role', 'sys_user_specific_permission', 'sys_workflow', 'sys_workflow_action', 'sys_workflow_state', 'sys_workflow_transition')
  AND column_name NOT IN
    ('case_id', 'case_1_id', 'case_2_id', 'created_by_session', 'deleted_by', 'email_thread_id', 'id', 'latest_snapshot_id',
    'parent', 'parent_id')
ORDER BY
  table_name,
  column_name;
```

- In a blank document, build up the sql statements that will be inserted into the virtual tables in yellowfin.

#### Multi-value fields
- Log into the data base and paste the following code to return the multi-value fields:

```
SELECT
  CONCAT('vw_',table_name) as table_name,
  column_name,
  data_type
FROM
  information_schema.columns
WHERE
  data_type IN ('ARRAY')
  AND LEFT(table_name,3)='sys'
  AND table_name NOT IN
    ('sys_assignment_rule', 'sys_dialing_instruction', 'sys_document', 'sys_emailrule', 'sys_escalation', 'sys_event', 'sys_grid_column', 'sys_grid_filter',
    'sys_holiday_entry', 'sys_link_match', 'sys_link_snapshot', 'sys_list', 'sys_listitem', 'sys_log', 'sys_message', 'sys_notification', 'sys_passwordhistory',
    'sys_permission', 'sys_person', 'sys_response', 'sys_role_filter', 'sys_role_permission', 'sys_searches', 'sys_session', 'sys_template', 'sys_todo_automation',
    'sys_translation', 'sys_trigger', 'sys_user_role', 'sys_user_specific_permission', 'sys_workflow', 'sys_workflow_action', 'sys_workflow_state', 'sys_workflow_transition')
  AND column_name NOT IN
    ('case_email', 'computed_search', 'content_searchable','exclude_from_purge',
    'investigative_team_members', 'files', 'attachments', 'failed_recipients', 'recipients', 'perm', 'service','sys_active','sys_processing','sys_submitted', 'system_blacklist', 'user_blacklist')
ORDER BY
  table_name,
  column_name;
``` 
- In yellowfin, add a virtual table for each of the multi-value field.

#### JSON fields
***ASK HOW TO DETERMINE WHICH FIELDS TO DO THIS TO**

### Export translations
- In the app code base, add a file called `script/translate-yf.js` and paste the following code in it:
```
/* eslint-disable no-console */
const _ = require('lodash');
const fs = require('fs');
const reader = require('buffered-reader');
const knex = require('isight/services/db.js');

const outputData = [];
const missingData = [];

if (process.argv.length < 3) {
	console.log('ex. node script/translate-yf.js <input_file> <language>');
	process.exit(0);
}

const input = process.argv[2];
const locale = process.argv[3] || 'en_US';

knex('sys_translation')
	.select('value', 'key')
	.where('locale', locale)
	.then((translations) => {
		new reader.DataReader(input, { encoding: 'utf8' })
			.on('error', (error) => {
				console.log(error);
			})
			.on('line', (row) => {
				const rowData = row.split(',');
				const field = rowData.length >= 4 ? rowData[3] : null;
				const isValid = rowData && rowData[1] === 'VIEWFIELDNAME'
				&& field && field.indexOf(' ') === -1 && !/[A-Z]/.test(field[0]);
				if (isValid) {
					const translation = _.result(_.find(translations, t => t.key === _.camelCase(field).toLowerCase()), 'value');
					if (translation) {
						if (rowData.length >= 5) {
							rowData[4] = `"${translation}"`;
						} else {
							rowData.push(`"${translation}"`);
						}
					} else {
						missingData.push(field);
					}
				}
				outputData.push(rowData);
			})
			.on('end', () => {
				const stream = fs.createWriteStream(input);
				stream.once('open', () => {
					_.each(outputData, (line) => {
						stream.write(`${line.join()}\n`);
					});
					stream.end();
				});
				console.log(' >>', input, 'updated.');
				console.log('No translations were found for the following fields:\n', missingData.join(', '));
			})
			.read();
	});

```
- Run the file with this command `node script/translate-yf.js <path_to_translation_file>.csv`
- Then upload the file to yellowfin

#### Yellowfin variables in Jenkins
The config envars are to be added once yellowfin is setup.

```
YF_ENABLED=true
YF_URL=https://report<number>.i-sight.com
YF_URL_SVC=https://report<number>.i-sight.com
YF_USER=
YF_PASSWORD=
YF_ORG_ID=1
YF_ORG=<customer_name>
YELLOWFIN_IP=10.0.10.250
YELLOWFIN_PORT=80
YF_DB_USER=yellowfin
YF_DB_PASS=y3llowfin
DISABLE_DEVOPS_PING=true
```

***
[Table of Contents](../README.md)