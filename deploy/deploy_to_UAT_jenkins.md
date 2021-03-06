## Deploy to UAT - Jenkins

Open Word document from IT ticket for the project. It can be found in your emails by searching the project name. IT would have created the environment. Click on "View ticket" which will take you to the service desk where you can view the word document.

### Deployment process
- log into Jenkins
- navigate to Team Pro > non-Prod (UAT)
- find and click on the project name
- click on `<name>-uat-deploy-v1.0`
- click on 'Build with Parameters' from left navigation pane
- Fill in these fields:
  - SAFETY_CHECK: The 'DBASE' INTERNAL IP address (from Word doc)
  - DESCRIPTION: Provide description ex: Initial deploy
  - GIT_TAG: provide tag for release version (ex. v1.0.0)
  - BYPASSS_TAG: un-check box (if tag already exists on github, check the box).
  - GIT_BRANCH: Set this to 'master'. 
  - TASK: 'Other'
  - POSTRUN_SCRIPT: See notes below
- Click 'Build' once all information is inputted.
- Click on project name in the path banner
- Click on `<project_name>-uat-deploy-<version>` tab to see deploy progress.
- Once deploy is complete, navigate to the UAT URL and test the app.
- In Jira, mark the ticket 'Ready for QA'

### Re-deployment
Somethings are different if redeploying to UAT.
- Log into Jenkins
- navigate to Team Pro > non-Prod (UAT)
- Choose the project to be deployed and click on the 'Name'
- Click on on project
- Click 'Rebuild Last' on left navigation bar
- Change fields that you want to change (i.e. GIT_TAG)
- Click 'Rebuild' once all information is inputted.
- Click on project name in the path banner
- Click on `<project_name>-uat-deploy-<version>` tab to see deploy progress.
- Once deploy is complete, navigate to the UAT URL and test the app.
- In Jira, mark the ticket 'Ready for QA'
***

### SERVER_ENVARS
Add the following envar to the deployment:
`ALLOWED_REFERERS=<project_name>.i-sightuat.com`

### POSTRUN_SCRIPT - Initial
```
export NODE_ENV=development
make breakdown
make setup
make create-sample-users
```

### POSTRUN_SCRIPT - Non-initial
```
export NODE_ENV=development
make users-export
make breakdown
make setup
make create-sample-users
make users-import
```

***
[Table of Contents](../README.md)