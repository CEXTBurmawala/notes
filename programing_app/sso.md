## Configure SSO

Single sign-on configuration.

### metadata file to send to customer
A metadata file is to be created with the customer's isight url added to it. This file is then sent to the customer for their IT department to work with.

### [^5.4.x App] Note
The sso configuration is already implemented in the app. Only the environment variables need to be set.

### [5.4.x App] SERVER_ENVARS in Jenkins

project_name
- This is the project name from the url

identity_provider
- Find the `SingleSignOnService` tag and copy the `location` variable which should be a URL that ends in SAML.

domain
- This can be be multiple domains.
- Add the `entityID` URL from the `EntityDescriptor` tag. Only add the base URL
- Add the base URL from the identity_provider URL.

cert
- This is the `X509Certificate` where it says `<KeyDescriptor use="signing">`

```
LOG_SSO=true
SAML_TYPE=wsfed
WSFED_AUTH_ENABLED=true
ALLOWED_REFERERS=<project_name>.i-sightuat.com,<domain>
WSFED_IDENTITY_PROVIDER_URL=<identity_provider>
WSFED_REALM=https://<project_name>.i-sightuat.com
WSFED_CERT=<cert>
```

### References

- [local testing](https://i-sight.atlassian.net/wiki/spaces/DKB/pages/696025112/Testing+SSO+Locally)
- [atlassian documentation](https://i-sight.atlassian.net/wiki/spaces/DTG/pages/108989269/SSO+Documentation)
- [Sample SSO config](https://github.com/i-Sight/config_umd_v5/commit/55435b5d2d17f712625f2f1fe2f4e4b5676e7992)
- [Nnamdi's notes](https://github.com/CEXNIbe/ReadMe/wiki/SSO-Setup)


***
[Table of Contents](../README.md)