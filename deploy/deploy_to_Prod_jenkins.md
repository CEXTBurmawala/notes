## Deploy to Prod - Jenkins

### Deployment process
- Log into Jenkins
- navigate to Team Pro > non-Prod (UAT)
- Choose the project to be deployed and click on the 'Name'
- Click on 'Build with Parameters' from left navigation pane
- Fill in these fields:
	- SAFETY_CHECK: The 'DBASE' INTERNAL IP address
	- DESCRIPTION: Provide description ex: initial deploy
	- GIT_TAG: provide tag for release version (ex. v1.0.0)
	- BYPASSS_TAG: un-check box.
	- GIT_BRANCH: Set it to 'master'.
	- TASK: 'Other'
	- POSTRUN_SCRIPT: See notes below
- Click 'Build' once all information is inputted.
- Click on project name in the path banner
- Click on `<project_name>-???-deploy-<version>` tab to see deploy progress.
- Once deploy is complete, navigate to the URL and test the app.
- In Jira, mark app as 'Ready for QA' ???


### POSTRUN_SCRIPT
```
make migrate
make sync-translations
???
```

### Notes on releases
- In github repo, click on 'releases'

***
[Table of Contents](../README.md)