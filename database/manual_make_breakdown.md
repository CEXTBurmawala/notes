## Manually Make Breakdown [5.x app]

Follow the steps outlined here to manually perform `make breakdown`.

The following databases need to be deleted:
```
	$(MAKE) db-delete $(DB_ENV)
	$(MAKE) db-delete $(AUDIT_DB_ENV)
	$(MAKE) db-delete $(FILESTORE_DB_ENV)
	$(MAKE) db-delete $(QUARTZ_DB_ENV)
	$(MAKE) es-delete-indices
	$(MAKE) remove-queues;
	$(MAKE) cache-clean;
```

Note: the `cache_clean` is the redis database.


### [1] Delete the postgres databases
- In Jenkins, build the db box in teleport
	<https://orchestration02.i-sight.com/job/team-pro/job/non-prod/job/Teleport-OpenSSHSession/>
- Log into teleport
- Run `docker ps` to see the container ID for the postgres container
- Run `docker exec -ti <container_id> bash`
- Run `dropdb isight -U postgres`
- Run `dropdb isight_audit -U postgres`
- Run `dropdb filestore -U postgres`
- Run `dropdb quartz -U postgres`

### [2] Delete elastic search indices
- In teleport, log in to the service box
- Run `docker ps` and copy the container ID for the `isight_server.1` container
- Run `docker exec -ti <container_id> bash`
- `./container-run.sh make breakdown DISABLE_DB_DELETE=true NODE_ENV=development DISABLE_ES_SNAPSHOT=true`

### [3] Remove queues
- ** MISSING **

### [4] Clear cache (redis)
- ** MISSING **

***
[Table of Contents](../README.md)
