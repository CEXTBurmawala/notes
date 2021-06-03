## Jenkins Boxes Down
If the Jenkins deployment fails in the second last box - `run-config`, the server or database boxes may be down.

To verify if the boxes are down, try to teleport into both of them, if they are down, the teleport link will return a 404 error.

If able to log into one of the boxes, run `docker node ls` and then `docker inspect <node_id>` to get information about the nodes.

***
[Table of Contents](../README.md)