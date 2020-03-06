## Network Containers

### Create a network:
`docker network create <network_name>`

### Connect container to a network:
`docker run -d --name=<name> --network=<network_name> <docker_image>`

### Disconnect a container from a network:
`docker network disconnect <network_name> <container_name>`

### List networks:
`docker network ls`

### Inspect network:
`docker network inspect <network_name>`

***
[Table of Contents](../README.md)