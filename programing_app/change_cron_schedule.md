## Change Cron Schedule

If the cron schedule for the FTP integration needs to be changed/updated, use the following steps to change it.

The change was made to the pro base to enable the deletion and re-creation of the cron schedule for the FTP integration:
- [pro base commit](https://github.com/i-Sight/config_pro_base_v5/commit/13aba99e87a52064e4ccd1d1ded380b25ca7a313)


#### Steps
[1] redeploy the app with the updated FTP envars
[2] build the teleport session in Jenkins for the service box
[3] in the service box, run `docker ps` and copy the container ID for the `isight_server.1...`
[4] run `docker exec -ti <container_id> bash`
[5] run the script `script/ftp-integration/reschedule-cron.js`


***
[Table of Contents](../README.md)