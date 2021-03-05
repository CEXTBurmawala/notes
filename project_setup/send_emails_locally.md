## Send Emails Locally

To send emails locally from inside the app, a smtp server needs to be setup and running locally.

#### [1] Install maildev
In the platform directory (`~/git/platform/5.5.x` for example), run `npm install -g maildev`

#### [2] Add envars
In the terminal window that has the node server, add the following envars:
```shell
export MAIL_TRANSPORT=false
export MAILSRV_PORT=1025
export MAIL_PORT=1025
```

#### [3] Add envars
In a third terminal window, run `maildev --ip 127.0.0.1`


### Note
The emails will be stored here: `http://localhost:1080/#/`

## Send email to the app from outside the app
Follow the steps below in order to send emails to the app locally:

#### [1] Setup email rule
In the app, under settings > system > email rules, create an email rule with the email address you'll be sending from, and the email address you want to send to (case.inbox@i-sight.com)

#### [2] Install swaks
The tool [swaks](https://www.jetmore.org/john/code/swaks/installation.html) allows you to send emails from the outside the app.

#### [3] Create the email
In your email app, send yourself an email with the content you want to sent to the app. Use the case number if you want to attach the email to a case directly. Then, save the email as a raw content file with the `.eml` file extension.

#### [4] Send the email to the app
In order to send the email that was exported, run the following command and include the path to the exported email.
`swaks -s localhost -p 1025 --from flavoie@i-sight.com --to case.inbox@i-sight.com -d - < path/to/file.eml`

***
[Table of Contents](../README.md)