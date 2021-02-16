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
- clone the pro base to the new project directory `git clone git@github.com:i-Sight/config_pro_base_v5.git config_<name>_v5`.
- Once cloned, `cd` into the project directory and change the remote repository to point to the new repo created for the project by running `git remote set-url origin git@github.com:i-Sight/config_<name>_v5.git`. This sets the 'origin' to the new project repo.
- Add the 'pro base' as 'base' in order to pull updates if required later on by running `git remote add base git@github.com:i-Sight/config_pro_base_v5.git`
- Run `git remote -v` to double check that the 'origin' and 'base' are set right.
- Open the project directory in VSCode and make the following changes to the 'package.json' file:
  - Change all instances of 'base' in the file to <project_name>
  - Change the version number to '1.0.0'
- The changes can now be pushed to the github repository:
  - `git checkout -b develop` to create the develop branch,
  - `git add .` to add all changes,
  - `git commit -m "Initial commit"` to make the initial commit,
  - `git push origin develop` to push the work to github. The default branch will now be 'develop'.
- Create the master branch and push it to github as well.
- Delete <v5.x.x> branch.
- Checkout the master branch and create a new working branch called 'PROBUILD-###'.
- There should now be 3 branches:
  - PROBUILD-###
  - develop
  - master
- In both the platform and project directories:
  - Run `nvm use` to set the node version to be used. (this will check the .nvmrc file and use the correct node version)
- In platform directory:
  - Run `yarn`
  - Run `yarn link`
  - Run `make docker-up`
- In project directory:
  - Run `yarn link isight`
  - Run `make install` (which will perform the following actions: `yarn`, `make build`, `make breakdown`, `make setup` and `make create-sample-users`)

### Setup project directory on local machine (if existing project)
- Ensure that the platform version is in the `~/git/_platform` directory (see package.json or platform.version file for platform version that the project is built on).
  - See **[this](./setup_new_platform_locally.md)** page for instruction on getting the correct platform version setup.
- Create a directory for the customer in `~/git/config_<name>_v5` directory with the same name as the git repo.
- Find repo on the i-sight github page `github.com/i-Sight/config_<name>_v5` and clone the 'develop' branch into the project directory.
- Create a new branch before starting to work on the project.
- In both the platform and project directories:
  - Run `nvm use` to set the node version to be used. (this will check the .nvmrc file and use the correct node version)
- In platform directory:
  - Run `yarn`
  - Run `yarn link`
  - Run `make docker-up`
- In project directory:
  - Run `yarn`
  - Run `yarn link isight`
  - Run a dev setup (if required) `make breakdown && make setup && make create-sample-users`

### Start up project
- In one terminal window:
  - Navigate to the project directory `~/git/config_<name>_v5`
  - Run `make watch`
- In another terminal window:
  - Navigate to the project directory `~/git/config_<name>_v5`
  - Run `node server.js`
- Navigate to <http://localhost:8000> in the browser.

### Potential issue
- If dev setup fails because it references another project, check the config path by running `echo $APP_CONFIG_PATH` and if it's not empty, run `unset APP_CONFIG_PATH`
- If dev setup fails because of 'schedule-purge.js', check Docker containers that Quartz is running.


***
[Table of Contents](../README.md)