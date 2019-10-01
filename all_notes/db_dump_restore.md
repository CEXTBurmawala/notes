## Database dump and restore

Run this command to dump data:
`pg_dump -h localhost -U postgres -d isight -t sys_user`

Add `> <file_name>` to the end

pg_dump: Dump data from the database
-h: flag for hostname (localhost)
-U: flag for username (postgres)
-d: flag for database name (isight)
-t: flag for table name


***
[Table of Contents](../README.md)