## Migrations General

Migrations are used to alter the database data and/or structure. The write a migration script, create a new file in `script/database/migrations/` and name the file with the next sequential number following the naming convention of the migrations scripts already in the directory. Migrations can be written as either a knex (js) file or a plain sql file.

Typical naming convention for knex js files:
- `001.knex.description-of-migration.js`

Typical naming convention for sql files:
- `001.do.description-of-migration.sql`

To run migration scripts:
- run `make auto-migrate`
- run `make es-reindex-data` and `make sync-data-dictionary` to reindex the fields in elastic search.
- run `make sync-data-dictionary` to sync the data dictionary.

#### Notes
- If the migration script doesn't work as intended, the migration needs to be removed from the 'schemaversion'
  - run `select * from schemaversion;`
  - the run `delete from schemaversion  where version='<number>';` type the number that you want to delete from the schema.


***
[Table of Contents](../README.md)