## Jenkins Deployment Issues
If the Jenkins deployment fails to complete the final step and the isight_worker box keeps crashing, redeploy the app with the following make command in the postrun: `make sync-queues`

If there is still an issue, you can remove and recreate the queues by logging into the service box and running the following commands:
- `make -C node_modules/isight remove-queues`
- `make -C node_modules/isight create-queues`

***
[Table of Contents](../README.md)