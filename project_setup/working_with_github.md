## Working with GitHub

### General
- Create a new branch when starting a project.
- Once features are implemented, push branch to GitHub and make a pull request.
- Paste link to pull request in 'pr-review' slack channel.
- Branch will be merged into 'develop' branch and feature branch can be deleted.
- On local machine, checkout the 'develop' branch and run `git pull	origin develop`
- Delete local feature branch.

### Editing Pull Requests
If changes are required in a pull request, here are the following steps to follow:
- Make the changes, commit changes and push changes to the branch. The pull request will be updated with the new changes and someone can review the PR again.
If others make changes, follow the same steps but you need to pull the recent commits pushed by other before making further changes.

### Merging to master branch
- Once all work is completed, the 'develop' branch can be merged into the 'master' branch for deploying to UAT or prod (production).
- Create pull request and merge 'develop' into 'master'.


***
[Table of Contents](../README.md)