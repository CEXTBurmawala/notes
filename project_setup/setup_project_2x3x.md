## Setup project v2.x & 3.x

- Ensure that the platform version is in the `~/git/_platform` directory (see package.json or platform.version file for platform version that the project is built on).
  - See **[this](./setup_new_platform_locally.md)** page for instruction on getting the correct platform version setup.
- Create a directory for the customer in `~/git/config_<name>_v5` directory with the same name as the git repo.
- Find repo on the i-sight github page `github.com/i-Sight/config_<name>_v5` and clone the 'develop' branch into the project directory.
- Create a new branch before starting to work on the project.
- In both the platform and project directories:
  - Run `nvm use` to set the node version to be used. (this will check the .nvmrc file and use the correct node version)
	- If the node version is not installed, run `nvm install <version_number>`
- Run `CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm link` in both the platform directory
- Run `CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install` in both the project directory


#### Branch off the platform + new shrinkwrap file
If packages need to be updated in the platform, a branch needs to be created off of the tag that.

- Create the new branch `git checkout -b <tag-number>-package-fix`
- Update the packages in the `package.json` file
- Delete the `npm-shrinkwrap.json`
- Run `rm -rf node_modules` and `npm cache clear`
- Run `CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm link` and ensure that there are not errors or conflicts
- Run `npm shrinkwrap --dev` which will create a new shrinkwrap file with the updated packages


#### Note
- If things go wrong during the `npm install`, run `rm -rf node_modules` and `npm cache clear`
- The `npm install` commands in both `Makefile` may need the additional flags in order to run


***
[Table of Contents](../README.md)