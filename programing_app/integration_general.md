## Integration General

Integration provides the customer the ability to connect their own, external data with the i-Sight app. User information can be searched when creating a new party and the information in the external CSV file will be auto-filled into the app fields as prescribed in the field mapping file (`/script/import-integration-parties/data-mapping.js`).

General notes:
- integration files are to be in CSV format
- the headers should be consistently formatted (cases type)


## Integration [5.x app]
Notes regarding integration setup can be found here:
[FTP.md](https://github.com/i-Sight/config_pro_base_v5/blob/v5.3.x/FTP.md)

#### add integration code to app
- **TO BE FILLED OUT**

#### testing integration locally
- set these envars locally:
	```
	export FTP_INTEGRATION_FOLDER_PATH=
	export FTP_INTEGRATION_ENTITY=
	export FTP_INTEGRATION_HOUR_OF_DAY=
	export FTP_INTEGRATION_DAY_OF_WEEK=
	export FTP_INTEGRATION_INTERVAL=
	```
- run `node script/ftp-integration/enable.js` to enable integration in the app
	- if script fails, set the `export APP_CONFIG_PATH=$(pwd)`
- log into the app and go to Settings > Workflows
	- in the dropdown menu, choose "Integration Mappings"
	- click on the "FTP Integration" in the table
	- click on the edit button
	- choose a unique identifier in the list and click save
- run `node script/ftp-integration/import.js` to import the integration data into the app

## Integration [4.x app]

#### Add integration code to app
- in `script/import-integration-parties/data-mapping.js`, add all the sample file fields and map them to the party fields. At the top of the file, a database table needs to be set as the integration table so that the app can populate the database table with the integration file rows.
- run the script `node script/import-integration-parties/generate-entity-files.js` in order to create
the entity for the new table.
- log into psql and check that the table was created.
- run a dev setup, run `make breakdown && make setup && make create-sample-users`
- run `pg_dump -h localhost -U postgres -d isight -t sys_employee --schema-only` in order to get the sql command in order to create a migration script that will be run in UAT/Prod.
	- ensure to choose the primary key in the script
- add any missing translations needed to the `en_US.js` file
- search for 'entityType' in the code and remove them

#### Testing integration locally
- set environment variables to run the integration locally:
	- `export INTEGRATION_LOCAL_IMPORT=true`
	- `export INTEGRATION_LOCAL_PATH=<path_to_sample_file>`
- run `node script/import-integration-parties/index.js` in order to import the rows from the csv file
into the app.

#### Test migration locally
- Checkout the previous release (git fetch / git checkout v#.#.#)
- Run a dev setup
- Checkout your feature branch
- Run `make migrate` and ensure that the scripts ran successfully.
- run `make sync-translations`
- Test the app locally to confirm that the integration still works.

#### if changes required to mapping
- delete all 'employee' related files
- re-run `node script/import-integration-parties/generate-entity-files.js`


***
[Table of Contents](../README.md)