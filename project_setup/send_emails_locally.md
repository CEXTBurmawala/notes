## Send Emails Locally

To send emails locally from inside the app, a smtp server needs to be setup and running locally.

### [1] Install maildev
In the platform directory (`~/git/platform/5.5.x` for example), run `npm install -g maildev`

### [2] Add envars
In the terminal window that has the node server, add the following envars:
```
export MAIL_TRANSPORT=false
export MAILSRV_PORT=1025
export MAIL_PORT=1025
```

### [3] Add envars
In a third terminal window, run `maildev --ip 127.0.0.1`


### Note
The emails will be stored here: `http://localhost:1080/#/`


***
[Table of Contents](../README.md)