## External Capture Modal

To set the onSaveClick event to a modal instead of a green banner notification (v5.x app):
- In the file `public/views/external-capture/external-capture-details-view.js` add the `modal: true` attribute to the `notify.display(...)` method. The `notify.js` file is a utility found in the platform under the `public/lib/` directory.

See [here](https://github.com/i-Sight/config_centracare_v5/blob/e78d3fe2ac9d73d9dda163f70932c6dbad90dd8a/public/views/external-capture/external-capture-details-view.js?ts=2) for an example.


#### Fix user role in database
If the app is already deployed and cannot be broken down, the following changes need to be made in the database which will add file upload as a user role for the anonymous user which will allow file uploads on external.

[1] Get the permission id from the sys_permissions table `select * from sys_permission where code='upload_file';`

[2] Get the user role id from the sys_user_role table `select * from sys_user_role;`

[3] Run the following command inserting the permission_id, user_role_id & new_uuid:
```
INSERT INTO "public"."sys_role_permission"("permission_id","user_role_id","user_lists","api_type","exclude_from_suggested_links","record_source","sys_processing","exclude_from_purge","date_purged","purge_reason","pending_purge_date","computed_search","sys_submitted","sys_active","deleted_by","deleted_date","last_updated_date","last_updated_by","created_date","created_by","id")
VALUES
(E'<permission_id>',E'<user_role_id>',NULL,NULL,FALSE,NULL,FALSE,FALSE,NULL,NULL,NULL,NULL,TRUE,TRUE,NULL,NULL,E'2020-07-09 13:06:39.251+00',NULL,E'2020-07-09 13:06:37.99+00',E'0',E'<new_uuid>');
```

[4] To verify that the role has been added, run `select * from sys_role_permission where user_role_id='<user_role_id>';`

***
[Table of Contents](../README.md)