## Update app to 5.4.0

There are a few options to update an existing app from 5.3 to 5.4.0. See different options below.

* IT needs a ticket to setup the upgrade for the app.

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
make auto-migrate
make sync-picklists
make sync-options
make populate-grids
make sync-permissions
make sync-permissions
make sync-themes
make sync-translations
make schedule-usage-stats-calculation
make es-reindex-data
make -C node_modules/isight sync-queues
make sync-permissions
```

#### 5.4 Release notes
Find release notes [here](https://i-sight.atlassian.net/wiki/spaces/DKBV5/pages/687276216/v5.4.0+-+Release+Notes#v5.4.0-ReleaseNotes-FixTasks)
***
[Table of Contents](../README.md)