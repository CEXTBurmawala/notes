## Deploy to UAT - nscale

### Release version update
- Merge 'develop' branch into 'master'
- On the project github repo, click on 'Releases' and click 'Draft a new release'
- Type the new version number **This should match the package.json version number*
- Set the target to 'master'
- Set the title to be the version number
- Mark the release as being 'pre-release' until it is released in prod
- Click 'Publish release'

### Note
- Running commands to modify/update/populate the database can be done before or after updating/deploying the app.

### Re-deployment [Azure]
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
  - A bash alias called `azure` runs the above command with the username
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE002`
  - The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Navigate to the project directory `cd deploy/<project_name>`
- Check how much space there is left, run `df -h` (confirm enough space available)
- Run `vim sys-config.json` and change the version number to match the one in the project (and github master)
- Run `make compile`
- See below about changing environment variables
- Run `cat workspace/config_<project_name>_v5/package.json` to confirm that the right release number was compiled
- Check slack channel to make sure no one else is building and to inform other that you're building
- Run `make build` to build the containers/app
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy` (confirm UAT/Prod)
- Run `make check` to ensure that no deviation between containers on service box
After deploying:
- Go into service box, run `make ssh`
- Run `docker ps` to verify that the containers are all running
  - Run `./container-run.sh make migrate DISABLE_DB_BACKUP=true`
  - Run `./container-run.sh make sync-picklists` [OPTIONAL]
  - Run `./container-run.sh make sync-translations` [OPTIONAL]
  - Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Navigate to project url `<project_name>.i-sightuat.com`
***

### Re-deployment [AWS]
- To log into bastion1, run `ssh -i key_name cex_user_name@ec2-34-214-84-10.us-west-2.compute.amazonaws.com`
  - A bash alias called `aws` runs the above command with the username
- To log into 'Nscale', run `ssh cexnscaleadmin@ip-172-31-38-173.us-west-2.compute.internal`
  - The password can be found on pleasant under Root > Pro > Environments > AWS > PAT/PRD/UAT > 'Nscale Stage'
- Navigate to the project directory `cd deploy/<project_name>`
- Check how much space there is left, run `df -h` (confirm enough space available)
- Run `vim sys-config.json` and change the version number to match the one in the project (and github master)
- Run `make compile`
- See below about changing environment variables
- Run `cat workspace/config_<project_name>_v5/package.json` to confirm that the right release number was compiled
- Check slack channel to make sure no one else is building and to inform other that you're building
- Run `make build` to build the containers/app
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy` (confirm UAT/Prod)
- Run `make check` to ensure that no deviation between containers on service box
After deploying:
- Go into service box, run `make ssh`
- Run `docker ps` to verify that the containers are all running
- Run `./container-run.sh make migrate DISABLE_DB_BACKUP=true`
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Run `./container-run.sh make sync-translations` [OPTIONAL]
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Navigate to project url `<project_name>.i-sightuat.com`
***





### Change environment variables
- From project directory, type `make ssh` to get into the service box
- Run `vim server.env`
- Check file for `DISABLE_DB_BACKUP=true`, if not there, add it.

### Data box
- From project directory, type `make ssh MACHINE=db` to get into the data box

### Clear the UAT box
- run `df -h` to see how much space is left on the box
- if the box is over 80% in use, it's a good idea to clear it.
- run `docker images -q | xargs docker rmi` Will remove all images on the server
- run `docker images` to see the list and select the numerical_id to be deleted 
- run `docker rmi <numerical_id>` to delete the images(s)
- run `df -h` to see how much space is used after deleting the images

### Go into container to see the files
- run `make ssh` to go into the service box
- run `docker ps` and copy the container ID you want (should be the one on port 8005)
- copy the container ID of the redis container and run `docker exec -it <container_id> bash`


***
### make breakdown in Azure
`make breakdown` doesn't work in azure. The tables need to be dropped manually. The following commands can be run in psql to drop the tables inside the service box.

- If users need to be dumped before making breakdown, see notes [here](../database/db_dump_restore.md)

- Run `psql -h <hostname> -U <username> --dbname=uat_<project_name>_isight`, then inside psql, paste these lines:
```
DROP TABLE core_schemaversion CASCADE;
DROP TABLE isight_entity CASCADE;
DROP TABLE isight_field CASCADE;
DROP TABLE schemaversion CASCADE;
DROP TABLE sys_attachment CASCADE;
DROP TABLE sys_case CASCADE;
DROP TABLE sys_case_link CASCADE;
DROP TABLE sys_comment CASCADE;
DROP TABLE sys_document CASCADE;
DROP TABLE sys_email CASCADE;
DROP TABLE sys_emailrule CASCADE;
DROP TABLE sys_emailtemplate CASCADE;
DROP TABLE sys_escalation CASCADE;
DROP TABLE sys_event CASCADE;
DROP TABLE sys_filter CASCADE;
DROP TABLE sys_grid_column CASCADE;
DROP TABLE sys_grid_filter CASCADE;
DROP TABLE sys_holiday_entry CASCADE;
DROP TABLE sys_listitem CASCADE;
DROP TABLE sys_log CASCADE;
DROP TABLE sys_note CASCADE;
DROP TABLE sys_notification CASCADE;
DROP TABLE sys_party CASCADE;
DROP TABLE sys_passwordhistory CASCADE;
DROP TABLE sys_response CASCADE;
DROP TABLE sys_searches CASCADE;
DROP TABLE sys_session CASCADE;
DROP TABLE sys_template CASCADE;
DROP TABLE sys_todo CASCADE;
DROP TABLE sys_translation CASCADE;
DROP TABLE sys_trigger CASCADE;
DROP TABLE sys_user CASCADE;
DROP TABLE sys_workflow CASCADE;
DROP TABLE sys_workflow_action CASCADE;
DROP TABLE sys_workflow_state CASCADE;
DROP TABLE sys_workflow_transition CASCADE;

```
- Then drop all remaining tables except the two `azure_superuser` owned tables.
- Then exit psql

- Run `psql -h <hostname> -U <username> --dbname=uat_<project_name>_isightaudit`, then inside psql, paste these lines:
```
DROP TABLE audit_log CASCADE;
DROP TABLE core_schemaversion CASCADE;

```
- Then exit psql
- Run `psql -h <hostname> -U <username> --dbname=uat_<project_name>_quartz`, then inside psql, paste these lines:
```
DROP TABLE core_schemaversion CASCADE;
DROP TABLE qrtz_blob_triggers CASCADE;
DROP TABLE qrtz_calendars CASCADE;
DROP TABLE qrtz_cron_triggers CASCADE;
DROP TABLE qrtz_fired_triggers CASCADE;
DROP TABLE qrtz_job_details CASCADE;
DROP TABLE qrtz_locks CASCADE;
DROP TABLE qrtz_paused_trigger_grps CASCADE;
DROP TABLE qrtz_scheduler_state CASCADE;
DROP TABLE qrtz_simple_triggers CASCADE;
DROP TABLE qrtz_simprop_triggers CASCADE;
DROP TABLE qrtz_triggers CASCADE;

```
- Then exit psql
- Run `psql -h <hostname> -U <username> --dbname=uat_<project_name>_filestore`, then inside psql, paste these lines:
```
DROP TABLE core_schemaversion CASCADE;
DROP TABLE file_uploads CASCADE;

```
- Then exit psql


#### Note:
Once the tables have been dropped, you can run:
`./container-run.sh make breakdown DISABLE_DB_DELETE=true NODE_ENV=development DISABLE_ES_SNAPSHOT=true`

After rebuilding the app, make a dev setup and if required, re-insert the users. `./container-run.sh make setup` (This will reset the database)


***
[Table of Contents](../README.md)