## Change mail host

- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
  - A bash alias called `azure` runs the above command with the username
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE002`
  - The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Navigate to the project directory `cd deploy/<project_name>`
- Go into service box, run `make ssh`
- Run `vim server.env` and change the following line: `MAIL_HOST=10.20.148.6`
- Run `cp container-run.sh start-container-run.sh` to copy the file
- Run `vim start-container-run.sh` to edit the file and delete all the contents and insert the script below.
- Run `./start-container-run.sh`
- Run `docker ps` and copy the 'CONTAINER ID' of the '0.0.0.0:8005->80000/tcp' image and run `docker logs -f <image_name>` to check that the front end is running.
- Run `cat server.env` to check that the MAIL_HOST is still correct and up to date. Get the URL and go to the website and ensure that the login page loads.

### Script
```bash
#!/usr/bin/env bash

docker kill $(docker ps | grep 'server-' | awk '{print $1}') > /dev/null
CONTAINER=`docker images | awk '{print $1}' | grep -v REPO | grep server | head -1`
source ./server.env

docker run -d --restart=always -v ${HOME}/data:/usr/local/data -v ${HOME}/cert:/etc/cert --env-file ${HOME}/server.env -e APPDYN_NODE_NAME=server0 -p 8000:8000 -p 2525:2525 -p 7000:8001 -p 9001:9000 ${CONTAINER} /bin/bash -c 'cd /isight; node server.js'

docker run -d --restart=always -v ${HOME}/data:/usr/local/data -v ${HOME}/cert:/etc/cert --env-file ${HOME}/server.env -e APPDYN_NODE_NAME=server1 -p 8005:8000 -p 9002:9000 ${CONTAINER} /bin/bash -c 'cd /isight; node server.js'

docker run -d --restart=always -v ${HOME}/data:/usr/local/data -v ${HOME}/cert:/etc/cert --env-file ${HOME}/server.env -e APPDYN_NODE_NAME=server2 -p 9003:9000 ${CONTAINER} /bin/bash -c 'cd /isight; node server.js'
```

***
[Table of Contents](../README.md)