## Multiple Integrations

#### 5.5.x Apps
In the 5.5.x apps, the only file that needs to be edited is the `config/options.fit-integration.js`. Add an object with the relevant information for each additional integration.

#### 5.4.x Apps
In order to add a second integration in a 5.4 app, most the files related to integration need to be duplicated and renamed to reflect what information they are integrating (for example, students and faculty). All the integrations will be merged into the person entity.

The FTP envars need to be updated to be specific to the integration, for example:
```shell
FACULTY_FTP_INTEGRATION_FOLDER_PATH=/usr/local/data/integration/faculty
FACULTY_FTP_INTEGRATION_ENTITY=person
FACULTY_FTP_INTEGRATION_HOUR_OF_DAY=17
FACULTY_FTP_INTEGRATION_INTERVAL=daily
STUDENT_FTP_INTEGRATION_FOLDER_PATH=/usr/local/data/integration/student
STUDENT_FTP_INTEGRATION_ENTITY=person
STUDENT_FTP_INTEGRATION_HOUR_OF_DAY=17
STUDENT_FTP_INTEGRATION_INTERVAL=daily
```

IT will need to setup two cron schedules to import each of the integration files into the correct folder.


See [this](https://github.com/i-Sight/config_howard_v5/commit/88d9ff6cf3cdee91b1a69d1874a512caa526a44a) commit for an example.

***
[Table of Contents](../README.md)