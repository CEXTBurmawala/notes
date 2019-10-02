## Change Project

### Shut down existing project
- In current platform directory:
	- Run `yarn unlink`
	- Run `make docker-down`
	- Run `docker ps` to ensure that containers are no longer running.
- In current project directory:
	- Shut down running server/webpack

### Use next node version
- In next platform directory:
	- Run `nvm use <0.0.0>`
- In next project directory:
	- Run `nvm use <0.0.0>`

### Start next project
- In next platform directory:
	- Run `yarn`
	- Run `yarn link`
	- Run `make docker-up`
- In next project directory:
	- Run `yarn`
	- Run `yarn link isight`
	- Run `make breakdown && make setup && make create-sample-users`
	- Run `node server.js`
	- Run `make watch`
- Navigate to <http://localhost:8000> in browser.

***
[Table of Contents](../README.md)