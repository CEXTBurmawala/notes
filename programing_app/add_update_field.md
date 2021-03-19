## Add/Update Field

In order to add a field to case/party/entity, follow the steps below. If the field adds a picklist, see the notes [HERE](./change_field_type.md) to add the picklists to the app.

### [5.x app]
#### [1] add field
The script `make auto-migrate` takes care of adding fields (as long as they are not new picklists fields)

#### [2] change field type
To change the field type (not to/from picklist type), check to see what the data type is in the database
- if they are both the same data type, running `make sync-data-dictionary` will take care of the change
- if they are different, a migration will need to be written. See [THIS](https://github.com/i-Sight/config_tamu_v5/blob/develop/script/database/migrations/002.do.make-first-name-searchtexbox.sql) commit for an example

#### [3] change field name/caption
To change a field name/caption, write a migration to rename the column name and caption.

**If `make auto-migrate` is used, the old column will be deleted, the new column will be created and any data in the old named column will be deleted**



***
[Table of Contents](../README.md)
