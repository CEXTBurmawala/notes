## AWS to Azure Migration

### Building the app
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE006`
  - The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Clone the nscale config master branch into the project folder `git clone git@github.com:i-Sight/nscale-config.git <project name>`
- `cd <project name>` and run `make setup`
- Edit the file `sys-config.json`
  - Edit 'sys-config.json' file with the proper name, namespace and id in the system object. The 'id' needs to be a unique id. Search online for UUID generator and use a newly generated id.
  - Edit the server tag in the server object and update the path to the github repository. It helps to look at another app for comparison.
	- The version number at the bottom depends on the app version (3.x and below use version 0.1, 4.x apps use 1.0)
```
{
  "system": {
      "name": "<project_name>",
      "namespace": "<project_name>",
      "id": "<generate_uuid>"
  },
  "containers": {
      "server": {
          "tag": "<set_tag>",
          "path": "git@github.com:i-Sight/config_<project_name>_v5.git"
      },
      "quartz": {
          "tag": "v1.1.2-vars-fix"
      },
  },
  "machines": {
    "defaults": {
      "nscale": {
        "minDiskSpaceGB" : "6.5"
      },
      "service": {
        "identityFile": "/home/cexadministrator/keys/uat-nscale-key"
             || "/home/cexadministrator/keys/prod-nscale-key",
        "user": "cexadministrator",
        "homePath": "/home/cexadministrator",
        "minDiskSpaceGB": "6.5"
      },
      "db": {
        "identityFile": "/home/cexadministrator/keys/uat-nscale-key"
             || "/home/cexadministrator/keys/prod-nscale-key",
        "user": "cexadministrator",
        "homePath": "/home/cexadministrator",
        "minDiskSpaceGB": "6.5",
        "esHeapSize": "512m",
        "ipDbAddress": "<database_hostname>"
      }
    },
    "uat": {
      "service": {
        "serverName": "<service_host_ip>",
        "certFile": "star_i-sightuat_com.pem",
        "keyFile": "i-sightuat_com.key",
        "ipAddress": "<service_host_ip>"
      },
      "db": {
        "ipAddress": "<data_host_ip>"
      }
    },
    "prod": {
      "service": {
        "serverName": "<service_host_ip>",
        "certFile": "star_i-sight_com.pem",
        "keyFile": "i-sight_com.key",
        "ipAddress": "<service_host_ip>"
      },
      "db": {
        "ipAddress": "<data_host_ip>"
      }
    }
  },
	"version": "0.1",
  "template": {
    "name": "azure"
  }
}
```

- After editing the file, quit out of the two vim windows which brings you to the `config.js` file. Paste these two lines at the top and delete the canary field line
	```
	identityFile: '/home/cexadministrator/keys/prod-nscale-key',
	user: 'cexadministrator'
	```
- Run `make validate ENV=prod`
- Run `make link`
- Copy these files from another project:
  - `cp -R ../<old_project_name>/workspace/config_<old_project_name>_v5/keys/ ./workspace/config_<project_name>_v5/keys`
  - `cp -R ../<old_project_name>/lib/plugins.js ./lib/`
  - `cp -R ../<old_project_name>/definitions/infrastructure.js ./definitions/`
	- in the `definitions/infrastructure.js` file, make sure that this flag is set to no under exports.redis: `redis-server --appendonly no`
	- in the `lib/plugins.js` file, make sure that on lines 97, 98 and 99 have the updated paths:
		- `/home/cexadministrator/data/elasticsearch`
		- `/home/cexadministrator/data/elasticsearchbackup'`
		- `/home/cexadministrator/data/redis'`
- Run `nscale system list` now we should see the app at the end of the list
- Run `make compile ENV=prod`
- ssh into the service box `make ssh ENV=prod`
	- Create the `server.env` file
	- Copy the contents of another app into the file and edit the information based on the data in Pleasant under "Aws to Azure"
	- Also take a look at the server.env info in the sharepoint folder for SSO and fpt information for the app you're building
	- Create the file `container-run.sh` and copy the contents from another app
	- Make the file executable, run `chmod +x container-run.sh`
- Run `make build ENV=prod`
- When it's done building, run `make preview ENV=prod` to see if it looks good

### Deploying the app
- Log into AWS NSCALE box, create a dump folder for the project on home `mkdir project_dumps`
- Dump the isight(project_isight), audit(project_isightaudit), quartz(project_quartz), and filestore(project_filestore) DBs (All info can be found in Pleasant for the environments) `pg_dump -h HOST -U USER -d DB_NAME > FILE_NAME_isight.sql`
- IT will move over the folder with these dumps to the new Azure environment
- Deploy the app,  run `make deploy` (confirm UAT/Prod)
- Import the four dump files to re-setup the DBs `psql -h HOST -U USER -d DB < file_path/file_name.sql`
- Log into the service box `make ssh ENV=prod` and run `make reindex-data`


### Reference
- [Nnamdi's notes](https://github.com/CEXNIbe/ReadMe/wiki/Azure-NScale-Version-1-Setup)
- [Confluence deployment notes](https://i-sight.atlassian.net/wiki/spaces/DKBV5/pages/23429130/V5+Deployment+Setup)

***
[Table of Contents](../README.md)