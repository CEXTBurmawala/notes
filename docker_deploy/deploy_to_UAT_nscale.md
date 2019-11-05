## Deploy to UAT - nscale

### Release version update
- Merge 'develop' branch into 'master'
- On the project github repo, click on 'Releases' and click 'Draft a new release'
- Type the new version number **This should match the package.json version number*
- Set the target to 'master'
- Set the title to be the version number
- Mark the release as being 'pre-release' until it is released in prod
- Click 'Publish release'

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
- Check slack channel to make sure no one else is building and to inform other that you're building
- Run `tmux ls` [OPTIONAL]
- Run `tmux new -s <project_name>` to start a new session [OPTIONAL]
- In the tmux session, run `make build` to build the containers/app
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy`
- Run `make check` to ensure that no deviation between containers on service box
- Exit tmux `exit`
- Navigate to the project directory `cd deploy/<project_name>`
- Go into service box, run `make ssh`
- Run `./container-run.sh make migrate DISABLE_DB_BACKUP=true`
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Run `./container-run.sh make sync-translations` [OPTIONAL]
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Navigate to project url (<project_name>.isightuat.com)


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
- Check slack channel to make sure no one else is building and to inform other that you're building
- Run `tmux ls` [OPTIONAL]
- Run `tmux new -s <project_name>` to start a new session [OPTIONAL]
- In the tmux session, run `make build` to build the containers/app
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy` (confirm UAT/Prod)
- Run `make check` to ensure that no deviation between containers on service box
- Exit tmux, run `exit`
- Navigate to the project directory `cd deploy/<project_name>`
- Go into service box, run `make ssh`
- Run `./container-run.sh make migrate DISABLE_DB_BACKUP=true`
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Run `./container-run.sh make sync-translations` [OPTIONAL]
- Run `./container-run.sh make sync-picklists` [OPTIONAL]
- Navigate to project url (<project_name>.i-sightuat.com)


### Change environment variables
- From project directory, type `make ssh` to get into the service box
- Run `vim server.env`
- Check file for `DISABLE_DB_BACKUP=true`, if not there, add it.

### Data box
- From project directory, type `make ssh MACHINE=db` to get into the data box

### To re-attach to a tmux session
- Run `tmux ls`
- run `tmux attach -t <session_name>`

### Notes
Running commands to modify/update/populate the database can be done before or after updating/deploying the app.


### make breakdown in Azure
`make breakdown` doesn't work in azure. The tables need to be dropped manually. The following commands can be run in psql to drop the tables:
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

DROP TABLE audit_log CASCADE;
DROP TABLE core_schemaversion CASCADE;

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

DROP TABLE core_schemaversion CASCADE;
DROP TABLE file_uploads CASCADE;
```

Then, once the tables have been dropped, you can run:
`./container-run.sh make breakdown DISABLE_DB_DELETE=true NODE_ENV=development DISABLE_ES_SNAPSHOT=true`

`./container-run.sh make setup`

This will reset the database 

***
[Table of Contents](../README.md)