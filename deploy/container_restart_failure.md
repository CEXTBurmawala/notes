## Container Restart Failure - Nscale

If the server containers fail to start and restart, check the logs by running `docker logs <container_id>` and seeing if seneca has terminated.

#### Steps

1. Run `make ssh` into the service box. If you are looking at the docker containers, you should already in the service box.
2. Stop and kill the offending containers, run `docker stop CONTAINER_ID` or `docker kill CONTAINER_ID`
3. Remove the images, run:

`docker images | grep 'ago' | awk '{print $3}' | uniq | xargs docker rmi -f` and 

`docker ps -a | grep 'ago' | awk '{print $1}' | uniq | xargs docker rm -f`

3. Now exit the service box, run `exit`
4. Remove the container images, run:

`docker images | grep 'ago' | awk '{print $3}' | uniq | xargs docker rmi -f` and 

`docker ps -a | grep 'ago' | awk '{print $1}' | uniq | xargs docker rm -f`

5. Stop nscale, run `nscale stop`
6. Remove the registry, run `rm -rf ~/.nscale/data/registry`
7. Start and login to nscale, run `nscale start` and `nscale login`
8. Now rebuild and redeploy the app

***
[Table of Contents](../README.md)
