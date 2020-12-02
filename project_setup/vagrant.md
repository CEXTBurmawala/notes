## VAGRANT

To build 2.x and 3.x apps locally, it may be easier to build them in vagrant. Follow the steps below to setup a vagrant dev environment.

### General environment setup
- create folder at `~/vagrant_box/app-<version>`
- create folders inside app folder called `docker-app` and `platform`
- copy Vagrantfile into app folder
- cd into app directory and run `vagrant up`
- run `vagrant ssh`
- cd into the `/vagrant`
- add `cd /vagrant` to the bottom of the `~/.bashrc` file
- install nvm, run `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
- run `source ~/.bashrc` and then `nvm install <node_version>`
- create super user password, run `sudo passwd root` and choose a password (123456)
- install the correct npm version, run `npm install npm@<npm_version> -g`
- run `sudo apt-get install build-essential`
- run `sudo apt-get install libcurl4-openssl-dev`
- add the isight specific envars to the `~/.bashrc` file

Note: you may want to revise the amount of memory and cpu cores allocated in VirtualBox to this vagrant box in order to get better performance.

- install docker
	- run `sudo apt update`
	- run `sudo apt install apt-transport-https ca-certificates curl software-properties-common`
	- run `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
	- run `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"`
	- run `sudo apt update`
	- run `apt-cache policy docker-ce` and confirm the output
	- run `sudo apt install docker-ce`
	- run `sudo systemctl status docker` to check that docker is running
	- run `sudo usermod -aG docker ${USER}` to enable permissions for run docker commands
	- run `su - ${USER}` and enter the su password
	- logout of vagrant and ssh back in
	- run `id -nG` to confirm docker was added
	- run `docker ps` to confirm permission
	- install docker-compose, run `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
	- run `sudo chmod +x /usr/local/bin/docker-compose`
	- run `docker-compose --version` to confirm that it is installed


- install psql
	- run `sudo apt-get install -y postgresql-client`
	- run `psql --version` to confirm it's installed

- install python 2
	- run `sudo apt install python2`



### Git setup
- create the file `./.gitconfig` and copy the user info from your computer
- cd into `~/.ssh`
- run eval `ssh-agent -s`
- run `ssh-keygen -t rsa -C flavoie@i-sight.com`
- run `cat ~/.ssh/id_rsa.pub` and copy the output
- on github.com under settings, add the ssh key



### Project setup
- clone the platform `git clone git@github.com:i-Sight/isight_main_v5_beta.git platform`
	- cd into platform and checkout the branch/tag that the project requires
	- run `npm install`
	- run `npm link`

- clone the platform (for docker) `git clone git@github.com:i-Sight/isight_main_v5_beta.git docker-app`
	- cd into docker-app and run `git co v4.2.7`
	- run `nvm install 6.11.5`
	- run `npm install`
	- run `sudo service postgresql status`, if it's active, run `sudo service postgresql stop`
	- run `make docker-up`
	- run `docker ps` to confirm all containers are up

- clone the app `git clone <app_ssh_url> <app_name>`
	- cd into the app directory and checkout the master branch
	- run `npm link isight`
	- run `make install`
	- run `node server` to startup the app
	- go to your browser and enter `localhost:8000` in the address bar


### Troubleshooting
- if the postgres container fails due to service running at port 5432, run `sudo service postgresql status` to see if the postgresql server is running. If it is, run `sudo service postgresql stop` and then bring the docker containers back up









