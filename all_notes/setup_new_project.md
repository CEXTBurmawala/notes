## Setup New Project

### Setup directory for wireframe documents
- Create a new directory in `~/documents/CUSTOMERS/<customer_name>` for the customer if they don't already have a directory.
- Create a sub-directory `~/documents/CUSTOMERS/<customer_name>/<issue_number>` with the JIRA issue number/name and copy the field spec documents into this directory.

### Setup project directory on local machine
- Ensure that the platform version is in the `~/git/_platform` directory (see package.json or platform.version file for platform version that the project is built on).
	- See [this](./setup_new_platform_locally.md) page for instruction on getting the correct platform version setup.
- Create a directory for the customer in `~/git/config_<name>_v5` directory with the same name as the git repo.
- Find repo on the i-sight github page `github.com/i-Sight/config_<name>_v5` and clone the 'develop' branch into the project directory.
- Create a new branch before starting to work on the project.
- Get required node version from the platform 'package.json' file.
- In both the platform and project directories:
	- Run `node -v` to check the node version being used.
	- Run `nvm use <0.0.0>` to set the node version to be used.
- In platform directory:
	- Run `yarn`
	- Run `yarn link`
	- Run `make docker-up`
- In project directory:
	- Run `yarn`
	- Run `yarn link isight`

### Start up project
- In one terminal window:
	- Navigate to the project directory `~/git/config_<name>_v5`
	- Run `make watch`
- In another terminal window:
	- Navigate to the project directory `~/git/config_<name>_v5`
	- Run `make breakdown && make setup && make create-sample-users`
	- Run `node server.js`
- Navigate to <http://localhost:8000> in browser.


***
[Table of Contents](../README.md)