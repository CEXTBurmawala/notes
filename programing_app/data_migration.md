## Data Migration

The following are the steps required to do a one-time migration of data.

#### [1] Files required
- sample data csv file
- mapping file

#### [2] Add code to the app
- add `migratedCaseNumber` field to the `entities/case/index.js` file
- add the field to the `public/models/case-model-ex.js`
- add the file `script/<name-of-migration>/data-import.js`
- add the file `lib/services/business-logic/data-integration/workflow-data-integration.js`
- add the file `lib/services/business-logic/data-integration/workflow/data-import-ex.js` which is a copy of the platform script which you can alter to prevent creating blank parties during the import
- add the file `config/options.data-import.js` which will contain the code to map the integration data from the csv file into the fields in the app

#### [3] add code to map fields
In the file `config/options.data-import.js`, write all the import code

#### [4] add missing fields
The fields that were added need to be created in the database. Run `make auto-migrate` to add them. 

#### [5] test the integration script
- add the following environment variables in your terminal before running the script:
	- `APP_CONFIG_PATH` is the path to the app code
	- `IN_HOUSE_MIGRATION_SRC_PATH` is the path to where the integration file is located
	- `IN_HOUSE_MIGRATION_FILE_NAME` is the integration file name
- run `node script/<name-of-migration>/data-import.js` to test the integration

When deploying, add the variables to the SERVER_ENVARS in Jenkins.

In deployment, `IN_HOUSE_MIGRATION_SRC_PATH=/usr/local/data/externaldata`

#### [6] deployment
Add the environment variables above to the SERVER_ENVARS section of the build

#### [7] run the integration
- run `node script/<name-of-migration>/data-import.js` to import the data
- run `make es-reindex-data` after importing the data


#### Reference
1- [Preferred Mutual](https://github.com/i-Sight/config_preferred_mutual_insurance_company_v5/pull/14/files) reference PR showing the code.

2- [George Brown](https://github.com/i-Sight/config_george_brown_college_v5/pull/39/files) reference PR showing the code.

***
[Table of Contents](../README.md)