## Remove fields
The command `make auto-migrate` will remove fields that are no longer in the entities and will add any new fields.

#### Renamed fields
If there are changes to the app where fields are renamed, the script `make auto-migrate` will add the renamed field as a new column in the `sys_case` table, and the old named field will be removed with a migration. Therefore, in order to rename a field, two sql queries need to be run:
```
ALTER TABLE sys_case RENAME COLUMN <old_name> TO <new_name>;
UPDATE sys_translation
	SET description='<new_field_caption>',
		value='<new_field_caption>',
		key='<new_field_name>',
		last_updated_date=CURRENT_TIMESTAMP
	WHERE key='<old_field_name>' AND subgroup_name='fields';
```

#### Notes
- the column names need to be carefully changed to snake case where the field name is truncated with a short UID at the end (see example commit). 
- the key is the field name in all lower case
- the description and value fields are the caption

#### Example

https://github.com/i-Sight/config_cwcpcn_v5/pull/23/commits/888949c4e45e89f8a413aedb9d79299ca576f8af

***
[Table of Contents](../README.md)