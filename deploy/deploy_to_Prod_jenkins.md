## Deploy to Prod - Jenkins

### Deployment process
- Log into Jenkins
- navigate to Team Pro > Prod
- Choose the project to be deployed and click on the 'Name'
- Click on 'Build with Parameters' from left navigation pane
- Fill in these fields:
	- SAFETY_CHECK: The 'DBASE' INTERNAL IP address (from Word doc)
	- DESCRIPTION: Provide description ex: initial deploy
	- GIT_TAG: provide tag for release version (ex. v1.0.0)
	- BYPASSS_TAG: un-check box  (if tag already exists on github, check the box).
	- GIT_BRANCH: Set it to 'master'.
	- TASK: 'Other'
	- POSTRUN_SCRIPT: See notes below
- Click 'Build' once all information is inputted.
- Click on project name in the path banner
- Click on `<project_name>-prod-deploy-<version>` tab to see deploy progress.
- Once deploy is complete, navigate to the URL to ensure it is running.
***

### POSTRUN_SCRIPT - Initial
```
export NODE_ENV=development
make breakdown
make setup
make create-sample-users
```


***
[Table of Contents](../README.md)