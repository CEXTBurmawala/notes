## Make Commands

To reset the password expiry:
- log into the database box
- connect to psql (see pleasant password manager)
- run this command: `UPDATE sys_user SET password_expiry='<future_date>' WHERE nick='isight';`


***
[Table of Contents](../README.md)