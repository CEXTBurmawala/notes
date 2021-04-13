## Migrations for picklists

When picklist items (new picklists or values in a picklist) are addd or updated and the database cannot be broken down, migration scripts need to be written to add the entries in the respective database tables 

note: new values will be added by the `make auto-migrate` script, including picklist values that are updated. The script won't update the database tables, only add the update values and keep the old ones as well. It's best to use a script to remove the values to be updated and let the auto-migrate script add the values with update names.

See [Uhhospitals](https://github.com/i-Sight/config_uhhospitals_v5/commit/7c47449702c1d3eae96047c15c8408159cb04a4b) for example migration scripts for picklists.

#### Migration for picklist to picklist[]
See jjay_cuny [migration 1](https://github.com/i-Sight/config_jjay_cuny_v5/blob/develop/script/database/migrations/003.do.update-fields-to-multi-picklists.sql) & [migration 2](https://github.com/i-Sight/config_jjay_cuny_v5/blob/develop/script/database/migrations/005.do.update-multi-picklists.sql) for the migrations to change a field from a picklist field to a multi picklist field.

***
[Table of Contents](../README.md)