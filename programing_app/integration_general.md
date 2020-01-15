## Integration General

Integration provides the customer the ability to connect their own, external data with the 
i-Sight app. User information can be searched when creating a new party and the information 
in the external CSV file will be auto-filled into the app fields as prescribed in the field 
mapping file (`/script/import-integration-parties/data-mapping.js`).

General notes:
- integration files are to be in CSV format
- the headers should be consistently formatted (cases type)
- 


### Testing the integration

To test the integration, a sample file is needed to ensure that the fields are 
auto-filled from the external sample data.

Once the changes are made to include the integration, the following commands need to be run:
- `export INTEGRATION_LOCAL_IMPORT=true`
- `export INTEGRATION_LOCAL_PATH=<path_to_sample_file>`
- `node script/import-integration-parties/`
- `make sync-data-dictionary && make es-reindex-data` [OPTIONAL]

***
[Table of Contents](../README.md)