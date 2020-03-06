## Clear lockout

#### [5.x app]
- build a teleport session into the db box
- run `docker ps`
- copy the container ID of the redis container and run `docker exec -it <container_id> bash`
- run `redis-cli`
- run `keys *lockout*`
- run `del <key>` with the key that you want to delete


***
[Table of Contents](../README.md)