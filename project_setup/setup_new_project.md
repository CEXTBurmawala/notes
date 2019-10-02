## Setup New Project

### Setup directory for wireframe documents
- Create a new directory in `~/documents/CUSTOMERS/<customer_name>` for the customer if they don't already have a directory.
- Create a sub-directory `~/documents/CUSTOMERS/<customer_name>/<issue_number>` with the JIRA issue number/name and copy the field spec documents into this directory.

### Setup new repo (if required)
- Ask Marvin to create repo on i-Sight github account <https://github.com/i-Sight> with the name provided from the ticket in Jira. Ask Rachel for the customer name to be used.
- give Rachel the repo name (should be `config_<name>_v5`) and the Docker name (`/<name>`).

### Setup project directory on local machine (if new project)
- Ensure that the platform version is in the `~/git/_platform` directory (see package.json or platform.version file for platform version that the project is built on).
	- See **[this](./setup_new_platform_locally.md)** page for instruction on getting the correct platform version setup.
- Create a directory for the customer in `~/git/config_<name>_v5` directory with the same name as the git repo.
- Find repo on the i-sight github page `github.com/i-Sight/config_<name>_v5` and clone the 'develop' branch into the project directory.
- clone the pro base from <https://github.com/i-Sight/config_pro_base_v5> to the new project directory. Ensure to clone the most recent branch (it should be the default branch).
- Once cloned, `cd` into the project directory and change the remote repository to point to the new repo created for the project by running `git remote set-url origin git@github.com:i-Sight/config_<name>_v5.git`. This sets the 'origin' to the new project repo.
- Add the 'pro base' as 'base' in order to pull updates if required later on by running `git remote add base git@github.com:i-Sight/config_pro_base_v5.git`
- Run `git remote -v` to double check that the 'origin' and 'base' are set right.
- Open the project directory in VSCode and make the following changes to the 'package.json' file:
	- Change all instances of 'pro_base' in the file to <project_name>
	- Change the version number to '1.0.0'
- The changes can now be pushed to the github repository:
	- `git checkout -b develop` to create the develop branch,
	- `git add .` to add all changes,
	- `git commit -m "Initial commit"` to make the inital commit,
	- `git push -u origin develop` to push the work to github.
	the default branch will now be 'develop'.
- Create the master branch and push it to github as well.
- Checkout the develop branch and create a new working branch called 'PROBUILD-###' and delete uneeded branches. There should now be 3 branches:
	- develop
	- master
	- PROBUILD-###
- Get required node version from the platform 'package.json' file.
- In both the platform and project directories:
	- Run `node -v` to check the node version being used.
	- Run `nvm use <0.0.0>` to set the node version to be used.
- In platform directory:
	- Run `yarn`
	- Run `yarn link`
	- Run `make docker-up`
- In project directory:
	- Run `yarn link isight`
	- Run `yarn`

### Setup project directory on local machine (if existing project)
- Ensure that the platform version is in the `~/git/_platform` directory (see package.json or platform.version file for platform version that the project is built on).
	- See **[this](./setup_new_platform_locally.md)** page for instruction on getting the correct platform version setup.
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
	- Run `yarn link isight`
	- Run `yarn`

### Start up project
- In one terminal window:
	- Navigate to the project directory `~/git/config_<name>_v5`
	- Run `make watch`
- In another terminal window:
	- Navigate to the project directory `~/git/config_<name>_v5`

	- Run a dev setup `make breakdown && make setup && make create-sample-users`
	- Run `node server.js`
- Navigate to <http://localhost:8000> in the browser.


***
[Table of Contents](../README.md)