## Setup new project

### Setup directories on local machine:
- Create a new directory in `Documents/CUSTOMERS` for the customer if they don't already have a directory.
- Create a directory with the JIRA issue number/name and copy the field spec documents into this directory.
- Ensure that the platform version is in the `git/_platform` directory (see package.json or patform.version file for platform version that the project is built on).
- Create a directory for the customer in `git/` directory with the customer name.
- Find repo on the i-sight github page and clone it into the project directory at `git/<customer_name>`.
- Run `yarn install` in terminal. /* ?? */
- Run `yarn link` in terminal. /* ?? */

### Adding platform version in `git/_platform` directory:
- Find repo on the i-sight github page and clone the platform into the platform directory.
	- Navigate to: <https://github.com/i-Sight/isight_main_v5_beta>
	- Click on branch and search for branch desired.
	- Clone branch into platform folder at `git/_platform/<3.x/4.x/5.x>`
- Run `yarn install` in terminal. /* ?? */
- Run `yarn link` in terminal. /* ?? */

### Link platform to project directory:
- ensure that both directories are using the correct node version (see package.json for version number). Type `nvm use x.xx.x` to use the desired node version.
- /* ?? */

### Setup Docker containers:
- /* ?? */

[Table of Contents](../README.md)