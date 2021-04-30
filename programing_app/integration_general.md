## Integration General

Integration provides the customer the ability to connect their own, external data with the i-Sight app. User information can be searched when creating a new party and the information in the external CSV file will be auto-filled into the app fields as prescribed in the field mapping file (`/script/import-integration-parties/data-mapping.js`).

General notes:
- integration files are to be in CSV format
- the headers should be consistently formatted (cases type)

Link to [Integration Setup - Jenkins](../deploy/integration_setup_jenkins.md)

## Integration [5.x app]
Notes regarding integration setup can be found here:
[FTP.md](https://github.com/i-Sight/config_pro_base_v5/blob/v5.4.x/docs/FTP.md)

#### add integration code to app
- Ensure that there is a field in the person entity that will be used as the  unique identifier and that there is a field in the CSV that will be mapped to the unique identifier.
- Ensure that there is a field in the person entity for each column in the CSV that the customer wants mapped.

#### testing integration locally
- set these envars locally:
  ```bash
  export INTEGRATION_ENABLED=true
  export FTP_INTEGRATION_FOLDER_PATH=/Users/flavoie/"OneDrive - i-Sight Software by Customer Expressions"/CUSTOMERS/<name>/integration
  export FTP_INTEGRATION_ENTITY=person
  export FTP_INTEGRATION_HOUR_OF_DAY=17
  export FTP_INTEGRATION_INTERVAL=daily
  ```
  - The folder path is the path to the sample integration file on your local machine. 
  - The entity should be either person or party
  - The hour of the day should be be between 1-23 (UTC time)
  - The interval should be either weekly or daily 
- run `node script/ftp-integration/index.js enable -a` to enable integration in the app
  - if script fails, set the `export APP_CONFIG_PATH=$(pwd)`
- log into the app and go to Settings > Workflows
  - in the dropdown menu, choose "Integration Mappings"
  - click on the "FTP Integration" in the table
  - click on the edit button
  - choose a unique identifier in the list and click save
  - match all the person fields to the correct integration file header
- run `node script/ftp-integration/index.js import -a` to import the integration data into the app

#### Multi picklists values
If some of the fields map to a multi-picklist (`picklist[]`) field, add the following code to the ` lib/integration/ftp-integration.js`
https://github.com/i-Sight/config_mnsu_v5/commit/bed36ec59282fe6f114b543183ea75bccf92e3a2

#### Changing the delimiter
If the delimiter in the csv file is not going to be a comma, the delimiter can be specific in the import script found in the file `lib/integration/ftp-integration.js`. The delimiter is specific in the csvParser variable.

#### Files with BOM character
If the files is encoded with a BOM character, the csv parser might have issues parsing the file. Therefore, the parameter `bom: true` can be added to the file `lib/integration/ftp-integration.js` in the csvParser variable.

To determine if the files has a BOM character, run the following command: `file <file_name>`. Note that not all files with BOM need to have this parameter set to true.

## Integration [4.x app]

#### Add integration code to app
- in `script/import-integration-parties/data-mapping.js`, add all the sample file fields and map them to the party fields. At the top of the file, a database table needs to be set as the integration table so that the app can populate the database table with the integration file rows.
- run the script `node script/import-integration-parties/generate-entity-files.js` in order to create
the entity for the new table.
- run a dev setup, run `make breakdown && make setup && make create-sample-users`
- run `pg_dump -h localhost -U postgres -d isight -t sys_employee --schema-only` in order to get the sql command in order to create a migration script that will be run in UAT/Prod.
- add any missing translations needed to the `en_US.js` file
- search for 'entityType' in the code and remove them
- commit the changes and checkout the previous branch and do a dev setup
- restore the pg_dump in the data base
- run the import script `node script/import-integration-parties/index.js`

#### Testing integration
- set environment variables to run the integration locally:
  - `export INTEGRATION_LOCAL_IMPORT=true`
  - `export INTEGRATION_LOCAL_PATH=<path_to_sample_file>`
- run `node script/import-integration-parties/index.js` in order to import the rows from the csv file
into the app.

#### If changes required to mapping
- make changes to the mapping file
- delete all 'employee' related files
- re-run `node script/import-integration-parties/generate-entity-files.js`


#### References
See the following PRs for the changes that were implemented to add integration:
- https://github.com/i-Sight/config_tamu_v5/pull/18/files?ts=2


***
[Table of Contents](../README.md)
