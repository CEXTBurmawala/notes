## Deploy to UAT - nscale

### Release version update
- Merge 'develop' branch into 'master'
- On the project github repo, click on 'Releases' and click 'Draft a new release'
- Type the new version number !! This should match the package.json version number
- Set the target to 'master'
- Set the title to be the version number
- Mark the release as being 'pre-release' until it is released in prod
- Click 'Publish release'

### Re-deployment [Azure]
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
	- A bash alias called `azure` runs the above command with the username
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE002`
	- The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Navigate to `deploy/<project_name>`
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
- Run `./container-run.sh make sync-translations` [OPTIONAL]
- Navigate to project url (<project_name>.isightuat.com)


### Re-deployment [AWS]
- To log into bastion1, run `ssh -i key_name cex_user_name@ec2-34-214-84-10.us-west-2.compute.amazonaws.com`
	- A bash alias called `aws` runs the above command with the username
- To log into 'Nscale', run `ssh cexnscaleadmin@ip-172-31-38-173.us-west-2.compute.internal`
	- The password can be found on pleasant under Root > Pro > Environments > AWS > PAT/PRD/UAT > 'Nscale Stage'
- Navigate to `deploy/<project_name>`
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

***
[Table of Contents](../README.md)