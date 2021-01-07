## Using Yellowfin Generator Setup

The following notes will enable you to generate the yellowfin configuration rather than manually creating the tables in yellowfin. The process involves starting a config, exporting it, running it through the generator along with the fieldspec, then importing it back into yellowfin.

#### [1] Create initial config
- start building the yellowfin system based on the guide, following the steps until the 'views' section
- for the database source, use the following:
	- name: <client_name> Prod
  - database type: postgres
  - database host: from the Word document, database internal IP
  - database port: leave is is (5432)
  - database name: isight
  - username: postgres
  - password: postgres

- once you start the view, drag the `vw_sys_case` to the entity relationship
- in the tables options, columns, select the `case_number`
- go to Prepare section at the top left
- at the bottom left, select the "+" button and click "Add / Edit Folders"
- add a "Case" folder
- drag the `case_number` from the left "VW_SYS_CASE" folder to the right "Case" folder
- Publish the view, enter the following information:
	- View Name: `<customer_name> Master View`
	- Description: `<customer_name> Master View`
	- Folder: `<customer_name> Reporting`
	- Sub-folder: `Views`

#### [2] Export the config
- after the view is published, go to the admin sidebar panel and click "Export"
- in the export process, move the `<customer_name> Master View` to the export list
- click on the blue "Export" button on the top left
- name the export what the app name is on github and click "Export"
- delete the master view from the Browse Views
- delete the data source from Admin console > data Sources

#### [3] Generate to complete config
- in a new console window, navigate to `~/vagrant_box/yf_generator` and run `vagrant up` and `vagrant ssh`
- clone the project repo
- copy the fieldspec and the exported YF view into the top level folder of the repo
- run the following command to generate the completed view:
	- `product-generator yellowfin-export ./<app_name>.xlsx ./<app_name>.yfx ./<app_name>-complete.yfx`

#### [4] Import the config
- log back into yellowfin
- delete the master view from the Browse Views
- delete the data source from Admin console > data Sources
- in the admin side panel, click on "Import"
- upload the `<app_name>-complete.yfx` file and choose "custom" option
- drag the "<app> Master View" and "<app> Prod" (database logo)
- click on the blue "Import" button at the top left

#### [5] Add filters
- once imported, finish all the steps in the Yellowfin Setup Guide after the Views section.


Reference: [official generator notes](https://github.com/i-Sight/isight-self-service/blob/master/docs/yellowfin.md)
online portal: [autobot](https://autobot.i-sight.com)

***
[Table of Contents](../README.md)
