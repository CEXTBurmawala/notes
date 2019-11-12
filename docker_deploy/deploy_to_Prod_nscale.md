## Deploy to Prod - nscale

It is good practice to `make build` the night before the push, and then `make deploy` in the morning during the prod push window.

### Deployment process


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