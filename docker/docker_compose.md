## Docker Compose

Docker Compose is based on a docker-compose.yml file. This file defines all of the containers and settings you need to launch your set of clusters.

Once the docker compose file is complete, run `docker-compose -f <file_path/file_name> up -d --build` to build the containers. The '-f' flag signifies a file path and name.

To see the details of the launched containers you can use `docker-compose ps`

To access all the logs via a single stream you use `docker-compose logs`

Run `docker-compose` to see all available commands and flags.

***
[Table of Contents](../README.md)