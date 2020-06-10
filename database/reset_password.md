## Reset Password

#### Reset the password expiry
- log into the database box
- connect to psql (see pleasant password manager)
- run this command: `UPDATE sys_user SET password_expiry='<future_date>' WHERE nick='isight';`

#### Reset after too many failed login attempts
- log into the DB box
- run `docker exec -it <REDIS_CONTAINER_ID> bash`
- run `redis-cli`
- run `keys *` to get a list of all the keys. Look for 
- run `get sys_lockout_isight` (or something similar)
- copy the value and change the tries to 0
- run `set sys_lockout_isight "{\"entity$\":\"-/sys/lockout\",\"id\":\"isight\",\"tries\":0}"`

***
[Table of Contents](../README.md)