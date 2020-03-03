## Yellowfin General

Yellowfin can be setup once the application is deployed to production. Once yellowfin is setup, the Pro app can be re-deployed with the envars.

- See the service ticket word document for the hostname in the PRODUCTION table under YELLOWFIN
- The login information (YF_USER & YF_PASSWORD) is found in pleasant under Reporting > Yellowfin > Top Level Adminstration

See notes on setting up Yellowfin [here](https://github.com/CEXNIbe/ReadMe/wiki/v5-Yellowfin-Setup) and the walk-through pdf in documents.

#### Steps
- visit `https://report<number>.i-sight.com` to get started
- sign in with the credentials from pleasant
- for the most part, follow along with the pdf guide.
- take note of the fields that have `kind: 'editable'`

#### Yellowfin variables in Jenkins
The config envars are to be added once yellowfin is setup.

```
YF_ENABLED=true
YF_URL=https://report<number>.i-sight.com
YF_URL_SVC=https://report<number>.i-sight.com
YF_USER=
YF_PASSWORD=
YF_ORG_ID=1
YF_ORG=<customer_name>
YELLOWFIN_IP=10.0.10.250
YELLOWFIN_PORT=80
YF_DB_USER=yellowfin
YF_DB_PASS=y3llowfin
```

***
[Table of Contents](../README.md)