## Setup integration - Jenkins

Integration allows the customer to integrate information (profiles) into their app based on a CSV file of their profile data. The app will run a script that will integrate the information/data into the app.

NOTE: The app needs to be deployed before the integration can be setup.

Link to [Integration General](../programing_app/integration_general.md)

LInk to [Pro Base notes](https://github.com/i-Sight/config_pro_base_v5/blob/v5.5.x/docs/Integration.md)

#### In Jenkins
- navigate to Prod/non-prod, and click on "Teleport-OpenSSHSession"
- click on "Build with Parameters", pick the project in the dropdown list (database)
- click on "Build"
- click on the job in the "Build History" side bar
- click on "Console Output" and click on the SSH Session link which will bring you to Teleport
- enter the login information + 2FA token
- repeat all the steps above for "service" box and have both boxes open in separate browser tabs


These variables need to be added to the `SERVER_ENVARS` section of the build parameters:
```
INTEGRATION_ENABLED=true
FTP_INTEGRATION_FOLDER_PATH=/usr/local/data/externaldata
FTP_INTEGRATION_ENTITY=person
FTP_INTEGRATION_HOUR_OF_DAY=17
FTP_INTEGRATION_INTERVAL=weekly
FTP_INTEGRATION_DAY_OF_WEEK=1
```

* for <= 5.4.0 apps, `FTP_INTEGRATION_FOLDER_PATH=/usr/local/data/integration`

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
- run `node script/ftp-integration/index.js enable -a`

#### [3] In the app
- Open another browser tab and navigate to the app
- navigate to Settings > Workflow > click dropdown and choose "Integration Mappings"
- click on the FTP Integration
- edit the mapping and set the "unique identifier"

#### [4] In the service box
While still inside the docker container
- run `node script/ftp-integration/index import -a`

#### [5] In the app
- after integration scripts have been run, go back to the app and click on "profiles" and check
that the data has been imported.


### Logs
In order to check the integration logs when you don't have access to log into the app, log into the database and make queries to the `sys_event` table.

***
[Table of Contents](../README.md)
