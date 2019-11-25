## Deploy to Prod - nscale

It is good practice to `make build` the night before the push, and then `make deploy` in the morning during the prod push window.

### Deployment process
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE006`
	- The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Clone the nscale config master branch into the project folder <https://github.com/i-Sight/nscale-config>
- Run `make setup`
- See Nnamdi's notes for an example of all the changes that need to be changed <https://github.com/CEXNIbe/ReadMe/wiki/Azure-NScale-Version-1-Setup>
	- Edit 'sys-config.json' file with the proper name, namespace and id in the system object. The 'id' needs to be a unique id. Search online for UUID generator and use a newly generated id.
	- Edit the server tag in the server object and update the path to the github repository.
- After editing the file, quit out of the two vim windows which brings you to 'config.js'
	- Paste in the two lines from Nnamdi's notes
- Next, run `vim makefile` and change the 'ENV' field to 'Prod'
- Run `make validate`
- Run `nscacle system list`
- Run `make link`
- Run `nscacle system list` and now we should see the app at the end of the list
- Run `make compile`
- Copy the keys from another project in the `cp -R ../<old_project_name>/workspace/config_<old_project_name>_v5/keys/ ./workspace/config_<project_name>_v5/keys` 
	- Create a temp directory (see Nnamdi's notes) and run the commands after changing the variables in the names. Run the first 3 first, then the second 3.
	- Run `cd ../` and run `ls` to ee that the files were copied.
	- Once done, delete the temp directory
- Now edit the 'sever.env' file
- Run `df -h` to ensure there is enough space on nscale
- Run `make build`
- When it's done building, run `make preview` to see if it looks good
- Then, run `make deploy` (confirm UAT/Prod)

- Log into the service box and run the make commands
- Go into the service box and run `docker ps` and get the CONTAINER ID and run `docker logs -f <container_id>`

### Yellowfin
- For the 5.x apps, refer to Nnamdi's notes here: <https://github.com/CEXNIbe/ReadMe/wiki/v5-Yellowfin-Setup>
- For the 4.x apps, refer to Nnamdi's notes here: <https://github.com/CEXNIbe/ReadMe/wiki/Yellowfin-Setup-(7.3)>

### Re-deployment [Azure]
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE006`
	- The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT

### Re-deployment [AWS]
- To log into bastion1, run `ssh -i key_name cex_user_name@ec2-34-214-84-10.us-west-2.compute.amazonaws.com`
	- A bash alias called `aws` runs the above command with the username
- To log into 'Nscale Prod', run `ssh cexnscaleprodadmin@ip-172-31-28-140.us-west-2.compute.internal`
	- The password can be found on pleasant under Root > Pro > Environments > AWS > PAT/PRD/UAT > 'Nscale Stage'


***
[Table of Contents](../README.md)