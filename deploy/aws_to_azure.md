## AWS to Azure Migration

#### Building the app in Azure
- To log into bastion1, run `ssh -i ~/.ssh/azure <username>@cec-dmz-bastion1.eastus.cloudapp.azure.com`
- To log into nscale, run `ssh cexadministrator@CEC-MGMT-NSCALE006`
  - The password can be found on pleasant under Root > IT > Azure > Azure East US > NSCALE > PROD/UAT
- Clone the nscale config master branch into the project folder `git clone git@github.com:i-Sight/nscale-config.git <project name>`
- `cd <project name>` and run `make setup`
- Edit the file `sys-config.json`
  - Edit 'sys-config.json' file with the proper name, namespace and id in the system object. The 'id' needs to be a unique id. Search online for UUID generator and use a newly generated id.
  - Edit the server tag in the server object and update the path to the github repository. It helps to look at another app for comparison.
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
- Run `nscale system list` now we should see the app at the end of the list
- Run `make compile ENV=prod`
- ssh into the service box `make ssh ENV=prod`
	- Create the `server.env` file
	- Copy the contents of another app into the file and edit the information based on the data in Pleasant under "Aws to Azure"
	- Also take a look at the server.env info in the sharepoint folder for SSO and fpt information for the app you're building
	- Create the file `container-run.sh` and copy the contents from another app
	- Make the file executable, run `chmod +x container-run.sh`
- Run `make build ENV=prod`
- When it's done building, run `make preview` to see if it looks good


#### Reference
[Nnamdi's notes](https://github.com/CEXNIbe/ReadMe/wiki/Azure-NScale-Version-1-Setup)

***
[Table of Contents](../README.md)