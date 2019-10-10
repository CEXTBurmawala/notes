## Change Project

### Shut down existing project
- In current __platform__ directory:
	- Run `yarn unlink`
	- Run `make docker-down`
	- Run `docker ps` to ensure that containers are no longer running.
- In current __project__ directory:
	- Shut down running server/webpack

### Use next node version
- In next __platform__ directory:
	- Run `nvm use <0.0.0>`
- In next __project__ directory:
	- Run `nvm use <0.0.0>`

### Start next project
- In next __platform__ directory:
	- Run `yarn`
	- Run `yarn link`
	- Run `make docker-up`
- In next __project__ directory:
	- Run `yarn`
	- Run `yarn link isight`
	- Dev setup, Run `make breakdown && make setup && make create-sample-users`
	- Run `node server.js`
	- Run `make watch`
- Navigate to <http://localhost:8000> in browser.

### Potential issue
- If dev setup fails because it references another project, check the config path by running `echo $APP_CONFIG_PATH` and if it's not empty, run `unset APP_CONFIG_PATH`
- If dev setup fails because of 'schedule-purge.js', check Docker containers that Quartz is running.

***
[Table of Contents](../README.md)