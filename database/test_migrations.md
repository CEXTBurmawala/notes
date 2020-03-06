## Test Migrations

Testing migrations locally required checking out the tag of the currently deployed app version and then running the migration and testing that the entities that were changed by the migration work in the updated app.

#### Steps
[1] checkout the tag of the app version that is currently deployed. Run `git checkout vx.x.x`
[2] run `make breakdown && make setup && make create-sample-users` to create the database based on the schema of the checked out tag version.
[3] checkout the latest branch/commit which contain the migration scripts
[4] run the migration scripts by running `make auto-migrate`
[5] test the app by adding/modifying the entity(ies) that were altered by the migrations

***
[Table of Contents](../README.md)