## Database dump and restore

A database dump can be used to save data (users for example) before re-setting the database so that the data can be re-introduced to the database after re-deployment. This can be done during UAT in the case where many changes were made to the app and a dev setup needs to be done but the data need to be preserved.

See [pg_dump](https://www.postgresql.org/docs/9.6/app-pgdump.html) link for postgreSQL documentation.
See [pg_restore](https://www.postgresql.org/docs/9.6/app-pgrestore.html) link for postgreSQL documentation.
See [this](https://docs.microsoft.com/en-us/azure/postgresql/howto-migrate-using-dump-and-restore) for Azure documentation on performing a database dump and restore.

### Dump Data
- Run this command to dump data:
	- `pg_dump -h <hostname> -U <username> -d uat_<project_name>_isight --data-only --table="sys_user" --column-inserts  > users.sql` to the end of the line. (get hostname and username from pleasant)
- To view the data dumb, type `vim users.sql` or `nano users.sql` (nano seems to display the information a little cleaner).
- [OPTIONAL] Reset the database, with or without populating sample users (See deploy documents).
- Log into psql and run `select id, nick from sys_user;` to verify what users are present in the database.
- After re-setting the database, the users need to be added back into the database.
	- Run SQL File: `psql -h localhost -U postgres -d isight < /path/file.sql`
	- [ALTERNATIVELY] In the data dump file, select and copy the 'INSERT' lines. Then, log into psql and paste the lines making sure to check that there was no errors when inserting the lines into the database. You may need to paste them twice.
- ** May need to be run twice
- Finally, run `./container-run.sh make es-reindex-data` to reindex the elastic search.

### Restore Data
Data can be restored in two main ways, with the `pg_restore` command or by running `psql -h <host_name> -U <username> -d <database> < /path/to/dump/file.sql`
- sdf

#### Flags
-h: flag for hostname (localhost) \
-U: flag for username (postgres) \
-d: flag for database name (isight) \
-t: flag for table name \
--data-only: only dumps the data, not the schema \
--column-insets: dump data as individual insert statements



#### Working with sample users
If doing a data dumb locally where sample users created other users, there can be errors when inserting the users back into the database because the created by ID (foreign key) will not match the sample users created after the database reset.


#### backup Database in Live App
In order to backup the database of a live app, the database needs to be dumped and then restored after a setup.
- In the db box in teleport, exec into the postgres container (`docker exec -ti <container_id> bash`)
- Perform a `pg_dump > backup.sql` with the desired flags. The command will create a backup file inside the container
	- in order to download the dump file, exit the container and run the following command: `docker cp <container_id>:/backup.sql .`
- The app can now be redeployed in Jenkins with the updated app version  with `make auto-migrate` in the post-run scripts section which will run the migrations.


***
[Table of Contents](../README.md)
