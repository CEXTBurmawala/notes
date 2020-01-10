## Setup integration

Integration allows the customer to integrate information (profiles) into their app based on a CSV 
file of their profile data. The app will run a script that will integrate the information/data into the app.

**The app needs to be deployed before the integration can be setup.

#### In Jenkins
- navigate to Prod/non-prod, and click on "Teleport-OpenSSHSession"
- click on "Build with Parameters", pick the project in the dropdown list (database)
- click on "Build"
- click on the job in the "Build History" side bar
- click on "Console Output" and click on the SSH Session link which will bring you to Teleport
- enter the login information + 2FA token
- repeat all the steps above for "service" box and have both boxes open in separate browser tabs


These variables need to be added to the `COMPOSE_ENVARS` section of the build parameters (at the bottom):
```
FTP_INTEGRATION_FOLDER_PATH=/usr/local/data/integration
FTP_INTEGRATION_ENTITY=person
FTP_INTEGRATION_HOUR_OF_DAY=1
FTP_INTEGRATION_INTERVAL=weekly
FTP_INTEGRATION_DAY_OF_WEEK=1
```
***
### STEPS

#### [1] In the database box
- navigate into the integration folder by running `cd /var/isight/integration/`
- run `ls isight_backup` to confirm CSV file is there
	- if not, add csv data by creating a file and copy/pasting or upload a sample file
- if file is in `isight_backup` directory, move it to parent directory
	- run `mv isight_backup/<file_name>.csv .`

#### [2] In the service box
- run `docker ps` and copy the container_id of the `isight_server.1...`
- run `docker exec -it <container_id> bash`
- run `node script/ftp-integration/enable.js`

#### [3] In the app
- Open another browser tab and navigate to the app
- navigate to Settings > Workflow > click dropdown and choose "Integration Mappings"
- click on the FTP Integration
- edit the mapping and set the "unique identifier"

#### [4] In the service box
- run `node script/ftp-integration/import.js`

#### [5] In the app
- after integration scripts have been run, go back to the app and click on "profiles" and check
that the data has been imported.


***
[Table of Contents](../README.md)