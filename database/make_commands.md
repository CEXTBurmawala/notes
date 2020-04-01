## Make Commands

Short list with descriptions of common commands used when setting up or deploying projects.

#### make auto-migrate
`make auto-migrate` is a command in the pro base Makefile intended to facilitate migrating changes to the app when deploying a newer version of the app.

These are the commands that are run:
```
auto-migrate:
	$(MAKE) -C $(PLATFORM_PATH) backup
	$(MAKE) -C $(PLATFORM_PATH) remove-views
	$(MAKE) -C $(PLATFORM_PATH) db-migrate
	$(MAKE) -C $(PLATFORM_PATH) sync-data-dictionary
	node script/database/auto-migrate.js default
	node script/database/scrub-data-dictionary.js default
	$(MAKE) -C $(PLATFORM_PATH) create-database-views
	$(MAKE) -C $(PLATFORM_PATH) sync-options
	$(MAKE) -C $(PLATFORM_PATH) sync-translations
	$(MAKE) -C $(PLATFORM_PATH) es-reindex-data
	$(MAKE) -C $(PLATFORM_PATH) create-queues
```

- `auto-migrate.js`: This script will automatically add/remove tables and columns
- `scrub-data-dictionary`: This script removes fields from the entity stored in the database

#### make breakdown
Deletes all databases

#### make setup
- Creates databases
	- Creates the databases and runs platform scripts to create data-dictionary and run migrations

- syncs data-dictionary
	- Runs the script `node script/sync-data-dictionary.js`

- populate
	- Runs the following commands:\
	[sync-options,	populate-system-users, populate-translations, sync-picklist-types, sync-picklists, sync-permissions, sync-themes, populate-triggers, populate-user-roles, populate-grids, populate-notifications, populate-entity-associations, populate-layouts, populate-standard-responses, populate-config-app-custom]

#### make populate-lists
- Runs script: `node script/remove-picklists.js`
- Runs script: `node script/sync-picklists.js`

#### make sync-translations
- Runs script `node script/sync-translations.js`

#### make sync-picklists
- Runs script `node script/sync-picklists.js`

#### make populate-translations
- Runs script: `node script/remove-translations.js`
- Runs script: `node node script/sync-translations.js`

#### make create-sample-users
- Runs script `node script/create-sample-users.js`

#### make docker-up
- Runs the docker-compose to build the containers

#### make docker-down
- Takes down the containers based on the docker-compose file

***
[Table of Contents](../README.md)
