## Setup integration - Nscale

Integration allows the customer to integrate information (profiles) into their app based on a CSV file of their profile data. The app will run a script that will integrate the information/data into the app.

**The app needs to be deployed before the integration can be setup.

#### Step [Azure]
- To log into bastion1, run `ssh -i ~/.ssh/azure username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
	- A bash alias called `azure` runs the above command with the username
- To log into nscale, run:
	- `ssh cexadministrator@CEC-MGMT-NSCALE002` or 
	- `ssh cexadministrator@CEC-MGMT-NSCALE006`
	- The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Navigate to the project directory `cd deploy/<project_name>`
- Go into service box, run `make ssh`
- In the file `server.env`, add these two variables:
	- `INTEGRATION_LOCAL_IMPORT=true`
	- `INTEGRATION_LOCAL_PATH=/usr/local/data/integration`
- Run `crontab -e` to add a cron job to the app (select editor of choice).
	- Add the following at the bottom: `<crontab_schedule> /home/cexadministrator/container-run.sh node script/import-integration-parties/index.js > /dev/null`

	** cron job quick reference -> <https://www.adminschoice.com/crontab-quick-reference>

	** to figure out crontab schedule -> <https://crontab.guru>

- edit `container-run.sh` to mount the volume:
	- add `-v /home/cexadministrator/integration:/usr/local/data/integration` to the `docker run` command

#### General Notes
- 

***
[Table of Contents](../README.md)