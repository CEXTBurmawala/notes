## Integration Mapping [5.x apps]

In order to speed up testing the integration, an sql script can be written to map the integration CSV file fields to the person fields rather than manually mapping the fields each time the app is deployed. The script is to be run in psql and can be done in locally deployed apps, as well as UAT and Prod apps.

- Create an sql script to update the mapping fields in the `sys_integration_mapping` table in postgres. A sample of sql scripts can be found in `~/Documents/devTools/sample_sql_scripts`.
- In the postgres database, `isight`, run the script by running this command:
	`UPDATE sys_integration_mapping SET specification='<mapping_JSON_data>'`


***
[Table of Contents](../README.md)
