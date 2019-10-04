## Docker General

Each container is sand-boxed. If a service needs to be accessible by a process not running in a container, then the port needs to be exposed via the Host. Once exposed, it is possible to access the process as if it were running on the host OS itself.

By default, the host is mapped to port 0.0.0.0, which means all IP addresses. You can specify a particular IP address when you define the port mapping. for example: 127.0.0.1:6379:6379

Containers are designed to be stateless. Binding directories (also known as volumes) is done using the option -v <host_dir>:<container_dir>. When a directory is mounted, the files which exist in that directory on the host can be accessed by the container and any data changed/written to the directory inside the container will be stored on the host. This allows you to upgrade or change containers without losing your data.

### Basic commands

#### See containers currently running:
`docker ps`

This also shows the image used for each container, the friendly name, ID and the uptime.

#### See all containers (including stopped ones):
`docker ps -a`

#### See more details about a running container:
`docker inspect <friendly_name|container_id>`

#### Search for docker image:
`docker search <name>`

#### Launch a container:
`docker run <flag> <name>:<version_number>`

By default, Docker will run containers in the foreground. Adding '-d' flag will run the container in the background (detached). Docker will run the latest version of the image, in order to specify the version number required, add a colon and the version number required.

#### Save data to Docker host:
`docker run -d --name <name> -v <directory_path>:/data/<container>`

#### Build Docker image:
`docker build -t <name>:<version> <directory>`

The `-t <name>` gives the image a friendly name.

#### See images on local host:
`docker images`

#### View dockerfile:
`cat Dockerfile`

#### Query base on label:
`docker ps --filter "label=<label_key>=<label_value>"`

### Data volumes

Docker Volumes are created and assigned when containers are started. Data Volumes allow you to map a host directory to a container for sharing data.

This mapping is bi-directional. It allows data stored on the host to be accessed from within the container. It also means data saved by the process inside the container is persisted on the host.

#### Create a new volume:
`docker run  -v <path> --name <container_name>:/data -d <image_name> appendonly yes`

if the volume is to be read-only, the command would change as follows:

`docker run -v <container_name>:/data:ro`

### Restart policies

Docker considers any containers to exit with a non-zero exit code to have crashed. By default a crashed container will remain stopped.

Depending on your scenario, restarting a failed process might correct the problem. Docker can automatically retry to launch the Docker a specific number of times before it stops trying.

#### Restart container set number of times:
`docker run -d --name <container_name> --restart=on-failure:#`

Change '#' to the number of times you'll like Docker to try and restart the container.

You can ask Docker to always restart the containers.

#### Restart container set number of times:
`docker run -d --name <container_name> --restart=always`


***
[Table of Contents](../README.md)