## Update versions

The Platform and Probase should have the latest patch versions and they should match. Ensure that the platform is on the latest patch and then double check that the Probase is also on the latest

### Update version of Platform
- Run `git branch` which will provide the platform version being used.
- Alternatively, the 'package.json' file will list the version.
- Run `git fetch` which will fetch the changes
- Run `git tag | grep v0.0` to list all branches and find the latest one. Alternatively, go to <https://github.com/i-Sight/isight_main_v5_beta> and search tags for the latest version.
- Run `git checkout <v0.0.0>`
- Run `git submodule update --init --recursive`
- Run `yarn`
- Run `make docker-up`
- Run `yarn link`

### [4.x apps] Update version of project Probase
- In the app directory, locate the file `platform.version` and manually update the version number.

### [5.x apps] Update version of project Probase
- Find most up to date pro base version by going to `https://github.com/i-Sight/config_pro_base_v5`
  - Select the branch that corresponds to the version required (ex: v5.0.x)
  - Click on the 'package.json' files to find the latest patch version.
- Find version being used by looking at the `package.json` file.
- Make sure to have the probase configured as 'base' by running `git remote -v`
  - If not configured, Add the 'pro base' as 'base' in order to pull updates if required later on by running `git remote add base git@github.com:i-Sight/config_pro_base_v5.git`
- Run `git fetch base`
- Run `git merge --no-edit base/v5.x.x` (example: v5.3.x)
- If there are conflicts:
  - Run `git add -A` and fix any conflicts that may exist.
  - For the commit, run the following `git commit --no-edit`

### [5.x apps] Upgrading to a new app version
For example, upgrading a 5.3.1 app to 5.4.0

- Follow the same steps as above.
- Fix merge conflicts as described.
- run `yarn` 

***
[Table of Contents](../README.md)