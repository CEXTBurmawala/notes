## Deploy to UAT - nscale

### Release version update
- On the project github repo, click on 'Releases' and click 'Draft a new release'
- Type the new version number !! This should match the package.json version number
- Set the target to 'master'
- Click 'Publish release'

### Re-deployment
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE002`
	- The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Navigate to `deploy/<project_name>`
- Check how much space there is left, run `df -h` (confirm enough space available)
- Run `vim sys-config.json` and change the version number to match the one in the project (and github master)
- Run `make compile`
- Check slack channel to make sure no one else is building and to inform other that I'm building
- Run `tmux ls`
- Run `tmux new -s <project_name>` to start a new session
- In the tmux session, run `make build` to build the containers/app
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy`
- Exit tmux
- Go into service box, run `make ssh`
- Run `./container-run.sh make migrate`
- Navigate to project url (<project_name>.isightuat.com)

- Once done, navigate to 

### Change environment variables
- From project directory, type `make ssh` to get into the service box
- Run `vim server.env`

### Data box
- From project directory, type `make ssh MACHINE=db` to get into the data box

### To re-attach to a tmux session
- Run `tmux ls`
- run `tmux attach -t <session_name>`

***
[Table of Contents](../README.md)