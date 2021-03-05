## Dump field names and types 

If Rachel asks for a fields and field types dump from the database/app

#### ^ v4.x
- Connect to the database box in nscale
- Run the following command in the database:

```sql
SELECT
value AS "Field Name",
'{' || SUBSTR(group_name, 5) || '.' || key || '}' AS "Template Value"
FROM
sys_translation
WHERE
group_name IN ('sys/case', 'sys/note', 'sys/party', 'sys/todo')
AND
subgroup_name = 'fields'
ORDER BY
group_name,
value;
```

```sql
SELECT value AS "Field Name", '{' || SUBSTR(group_name, 5) || '.' || key || '}' AS "Template Value" FROM sys_translation WHERE group_name IN ('sys/case', 'sys/note', 'sys/party', 'sys/todo') AND subgroup_name = 'fields' ORDER BY group_name, value;
```

#### Older apps

- Get fields from the config.json files of each entity and make a list of all fields in this format:
```
caseDetails | case.caseDetails
incidentDate | case.incidentDate
```

***
[Table of Contents](../README.md)
