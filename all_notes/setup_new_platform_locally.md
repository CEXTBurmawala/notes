## Setup New Platform

### Major version not on local machine:
- Navigate to <https://github.com/i-Sight/isight_main_v5_beta> 
- Clone the 'develop' branch to the directory `~/git/_platform/<0.0.x>`.
- In terminal, navigate to the platform directory `~/git/_platform/<0.0.x>`
- Run `git branch` to see what branch the platform is in.
- Run `git tag | grep <version_number>` (example: 4.0) and find the last patch version for the next step.
- Run `git checkout <version_number>` (example: v4.0.8).
- Run `git branch` to ensure correct branch.
- Run `git submodule update --init --recursive`
- Run `yarn --update-checksums` to install all dependencies.

***
[Table of Contents](../README.md)