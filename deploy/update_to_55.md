## Update app to 5.5.0

Follow one of the two options below to upgrade an app to 5.5.0.

__Note:__ that the integration folder path has changed in the `docker-compose.yml` file. If the app is already deployed, attention may be required if the app uses the old folder path. The `docker-compose.yml` file may need to be edited to make the existing integration setup.


#### [1] UAT + delete all data
In UAT where no data needs to be saved, the app can be deployed with the following POSTRUN_SCRIPTS:
```
export NODE_ENV=development
make breakdown
make setup
make create-sample-users
```

#### [2] UAT + save user date
When saving the database data, the app needs to be upgraded with migration scripts if there are changes to the app itself (fieldspec changes).

Deploy the app with the following POSTRUN_SCRIPTS:

```
make sync-data-dictionary
make auto-migrate
make sync-options
make sync-picklists
make -C node_modules/isight sync-picklist-types
make sync-translations
make sync-permissions
make -C node_modules/isight es-create-scripts
make sync-queues
make update-phone-number-format
make es-reindex-data
make sync-queues
```

#### 5.4 Release notes
Find release notes [here](https://i-sight.atlassian.net/wiki/spaces/DKBV5/pages/859209744/v5.5.0+-+Release+Notes)
***
[Table of Contents](../README.md)