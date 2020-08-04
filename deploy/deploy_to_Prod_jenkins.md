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

### Usage Dashboard
- Enable the usage dashboard. Copy a previous sql script and revise the year and month of deployment as well as the number of seats and capacity. If deploying with current year and month, these two parameters can be omitted).

USAGE_START_YEAR=0000 \
USAGE_START_MONTH=00 \
USAGE_SEAT_LIMIT=5 \
USAGE_STORAGE_LIMIT=10

See [here](https://github.com/i-Sight/config_pro_base_v5/blob/4766c852c012c6dc7b4331dff0ed84de35bb646a/script/generate/usage-rule.js) and [here](https://github.com/i-Sight/config_pro_base_v5/wiki/Populate-Usage-Rule) for documentation for usage rule.

### Yellowfin
- Once an app is deployed to Prod, the yellowfin reporting should be setup.
- Once done, redeploy the app with the yellowfin envars.
- Log into the app > settings > user roles and edit & save the super user.
- To confirm that the users have yellowfin access, add "yellowfin username" to the grid and confirm that the usernames are present.


### Final prod push
Once prod is finalized and ready to go to support and the customer starts using the app, the following steps need to be carried out:
- Remove the isight2 account in the database by running the following sql command: `UPDATE sys_user SET deleted_date=now(), sys_active=false WHERE nick='isight2';`
- Then run this command to update the es index: `node node_modules/isight/script/es/reindex-data.js sys/user`
- Update the isight account password to `C3xlabadmin!`


***
[Table of Contents](../README.md)