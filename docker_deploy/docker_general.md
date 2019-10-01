## Docker General

### Basic commands

Each container is sandboxed. If a service needs to be accessible by a process not running in a container, then the port needs to be exposed via the Host. Once exposed, it is possible to access the process as if it were running on the host OS itself.

By default, the host is mapped to port 0.0.0.0, which means all IP addresses. You can specify a particular IP address when you define the port mapping. for example: 127.0.0.1:6379:6379

Containers are designed to be stateless. Binding directories (also known as volumes) is done using the option -v <host_dir>:<container_dir>. When a directory is mounted, the files which exist in that directory on the host can be accessed by the container and any data changed/written to the directory inside the container will be stored on the host. This allows you to upgrade or change containers without losing your data.

#### See containers currently running:
`docker ps`

This also shows the image used for each container, the friendly name, ID and the uptime.

#### See more details about a running container:
`docker inspect <friendly_name|containe_id>`

#### Search for docker image:
`docker search <name>`

#### Launch a container:
`docker run <flag> <name>:<version_number>`

By default, Docker will run containers in the foreground. Adding '-d' flag will run the container in the background (detatched). Docker will run the latest version of the image, in order to specify the versio number required, add a colon and the version number required.

#### Save data to Docker host:
`docker run -d --name <name> -v <directory_path>:/data/<container>`

#### Build Docker image:
`docker build -t <name>:<version> <directory>`

The `-t <name>` gives the image a friendly name.

#### See images on local host:
`docker images`

#### View dockerfile:
`cat Dockerfile`

***
[Table of Contents](../README.md)