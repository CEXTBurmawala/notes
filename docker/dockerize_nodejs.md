## Dockerize Node.js

### Create the dockerfile:

1. Specify base image to build from
2. Make a directory to use as the working directory
3. Specify the new directory is the working directory
4. Copy the package.json file into the working directory
5. Run `npm install` to install the dependencies
6. Copy all the source code into the working directory
7. Specify the port that the app will run on
8. Specify the command to start/run the app

Once the dockerfile is created, the image needs to be built and then a container can be launched.

- To build the image, run `docker build -t <name> .`
- To launch the container, run `docker run -d --name <container_name> -p <port>:<port> <image_name>`

### Sample dockerfile:
```docker
FROM node:10-alpine
RUN mkdir -p /src/app
WORKDIR /src/app

COPY package.json /src/app/package.json
RUN npm install

COPY . src/app
EXPOSE 3000

CMD [ "npm", "start" ]
```

### Ignore files/directories

Files and directories can be ignored from the build using a `.dockerignore` file which lists the files/directories to be ignored. The file `.git` and `node_modules` should be added to the `.dockerignore` file.

***
[Table of Contents](../README.md)