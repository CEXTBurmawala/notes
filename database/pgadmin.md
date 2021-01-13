## pgAdmin
Use the following instructions to setup pgAdmin and connect it to the local database

#### Connect to local database
- launch pgAdmin from the app menu and enter in your master password, or create your master password
- right-click on "Servers" in the left hand side menu bar
- click on Create > Server...
- give it a name ("local_server" for example)
- click on the "Connection" tab
- enter the host name "localhost"
- enter the username and password (both are "postgres")

You should now have access to the databases (filestore, isight, quartz, etc..)

#### Working with the tables
- to find the isight tables, click on the following arrows:
	-	local_server > Databases > isight > Schemas > Tables

***
[Table of Contents](../README.md)