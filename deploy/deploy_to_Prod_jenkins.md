## Deploy to Prod - Jenkins

### Deployment process
- Log into Jenkins
- navigate to Team Pro > non-Prod (UAT)
- Choose the project to be deployed and click on the 'Name'
- Click on 'Build with Parameters' from left navigation pane
- Fill in these fields:
	- SAFETY_CHECK: The database INTERNAL IP address
	- DESCRIPTION: Provide description ex: initial deploy
	- GIT_TAG: Choose tag for release version
	- BYPASSS_TAG: if checked, Jenkins will not create a release tag and GIT_BRANCH will need to be specified.
	- GIT_BRANCH: Only needs to be specified if BYPASSS_TAG is unchecked. Set it to 'master'.
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