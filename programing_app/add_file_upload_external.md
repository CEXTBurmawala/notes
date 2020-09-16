## Add File Upload on External Capture

The following commit has the code changes that need to be added manually in order to add file upload on the external capture form:
[HERE](https://github.com/i-Sight/config_ung_v5/commit/e192f1bd11bfc643d7d02f51588f336567ba54ee?ts=2). For the save external code, take the code from this file [HERE](https://github.com/i-Sight/config_centracare_v5/blob/3dce091a6d170d652999ad96f97af52ce2dcae7f/lib/services/business-logic/external-capture/workflow/external-capture-save.js)


#### Fix user role in database
If the app is already deployed and cannot be broken down, the following changes need to be made in the database which will add file upload as a user role for the anonymous user which will allow file uploads on external.

[1] Get the permission id from the sys_permissions table `select * from sys_permission where code='upload_file';`

[2] Get the user role id from the sys_user_role table `select * from sys_user_role where name='Anonymous';`

[3] Run the following command inserting the permission_id, user_role_id & new_uuid:
```
INSERT INTO "public"."sys_role_permission"("permission_id","user_role_id","user_lists","api_type","exclude_from_suggested_links","record_source","sys_processing","exclude_from_purge","date_purged","purge_reason","pending_purge_date","computed_search","sys_submitted","sys_active","deleted_by","deleted_date","last_updated_date","last_updated_by","created_date","created_by","id")
VALUES
(E'<permission_id>',E'<user_role_id>',NULL,NULL,FALSE,NULL,FALSE,FALSE,NULL,NULL,NULL,NULL,TRUE,TRUE,NULL,NULL,E'2020-07-09 13:06:39.251+00',NULL,E'2020-07-09 13:06:37.99+00',E'0',E'<new_uuid>');
```

[4] To verify that the role has been added, run `select * from sys_role_permission where user_role_id='<user_role_id>' and permission_id='<permission_id>';`

***
[Table of Contents](../README.md)