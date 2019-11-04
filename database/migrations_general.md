## Migrations General

Migrations are used to alter the database data and/or structure. The write a migration script, create a new file in `script/database/migrations/` and name the file with the next sequential number following the naming convention of the migrations scripts already in the directory.


#### Notes
- If the migration script doesn't work as intended, the migration needs to be removed from the 'schemaversion'
	- run `select * from schemaversion;`
	- the run `delete from schemaversion  where version='<number>';` type the number that you want to delete from the schema.


***
[Table of Contents](../README.md)