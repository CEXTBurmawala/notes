## Add File Upload on External Capture

The following commit has the code changes that need to be added manually in order to add file upload on the external capture form:
[HERE](https://github.com/i-Sight/config_ung_v5/commit/e192f1bd11bfc643d7d02f51588f336567ba54ee?ts=2) and [HERE](https://github.com/i-Sight/config_jjay_cuny_v5/commit/7967dd30036c274505a3e75eeb4989d47846d763)

For the save external code, take the code from this file [HERE](https://github.com/i-Sight/config_centracare_v5/blob/3dce091a6d170d652999ad96f97af52ce2dcae7f/lib/services/business-logic/external-capture/workflow/external-capture-save.js?ts=2)


#### Fix user role in database
The easiest way to accomplish this is by writing a migration. [HERE](https://github.com/i-Sight/config_houston_spca_v5/blob/develop/script/database/migrations/001.do.user-role-for-external-file-upload.sql) is an example of the migration that should be added.

#### Alternative - Fix user role in database
If the app is already deployed and cannot be broken down, the following changes need to be made in the database which will add file upload as a user role for the anonymous user which will allow file uploads on external.

[1] Get the permission id from the sys_permissions table `select * from sys_permission where code='upload_file';`

[2] Get the user role id from the sys_user_role table `select * from sys_user_role where name='Anonymous';`

[3] Run the following command inserting the permission_id, user_role_id & new_uuid:
```sql
INSERT INTO "public"."sys_role_permission"("permission_id","user_role_id","user_lists","api_type","exclude_from_suggested_links","record_source","sys_processing","exclude_from_purge","date_purged","purge_reason","pending_purge_date","computed_search","sys_submitted","sys_active","deleted_by","deleted_date","last_updated_date","last_updated_by","created_date","created_by","id")
VALUES
((select id from sys_permission where code='upload_file'),(select id from sys_user_role where name='Anonymous'),NULL,NULL,FALSE,NULL,FALSE,FALSE,NULL,NULL,NULL,NULL,TRUE,TRUE,NULL,NULL,CURRENT_TIMESTAMP,NULL,CURRENT_TIMESTAMP,E'0',E'<UUID>');
```

[4] To verify that the role has been added, run `select * from sys_role_permission where user_role_id=(select id from sys_user_role where name='Anonymous') and permission_id=(select id from sys_permission where code='upload_file');`

***
[Table of Contents](../README.md)