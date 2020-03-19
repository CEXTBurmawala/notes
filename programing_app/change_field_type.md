## Change Field Type

In order to change a field type from a textarea or textbox to a picklist, follow the steps below.

### [1] Update case index.js
In the file `entities/case/index.js` revise the picklist type:
```
{     
	field: '<field_name>',
	type: 'picklist',
	caption: '<field_name>',
	typeOptions: {
		picklistName: '<picklist_name>'
	},
	kind: 'editable'
},
```

### [2] Add picklist name to config
In the file `config/options.picklists.js` add the new picklist name
```
'<picklist_name>': {
	text: 'value',
	value: 'value'
},
```

### [3] Add picklist values
Add picklist data file: `data/lists/<picklist_name>.json`
```
[
	{
		"name": "<picklist_name>",
		"value": "<picklists_value>"
	},
	...
]
```

### [4] Add translation
In the file `data/translations/en_US.js` add the translation for the picklist name.


### [5] Update field type in sys_case table
In the project directory `script/database/migrations/`, add a migration in this format: `000.do.update-field-type.sql`

In the migration script, add the following:
```
ALTER TABLE sys_case
ALTER COLUMN <column_name> TYPE character varying(255);
```

### [6] Add entry to sys_translation table
In the project directory `script/database/migrations/`, add a migration in this format: `000.do.add-translation.sql`
```
INSERT INTO sys_translation (...)
VALUES (..);
```

### [7] Add entry to sys_listitem table
In the project directory `script/database/migrations/`, add a migration in this format: `000.do.add-listitem.sql`
```
INSERT INTO sys_listitem (...)
VALUES (..);
```

### Example
[Part 1](https://github.com/i-Sight/config_ucla_health_v5/commit/0058495114051a786635e7ab991eeaa15871da5f)

[Part 2](https://github.com/i-Sight/config_ucla_health_v5/commit/23403c0b66509e62610a8d9314e5558fc689c659)

***
[Table of Contents](../README.md)
