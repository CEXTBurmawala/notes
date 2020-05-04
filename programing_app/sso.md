## Configure SSO

Single sign-on configuration.

### metadata file to send to customer
A metadata file is to be created with the customer's isight url added to it. This file is then sent to the customer for their IT department to work with. There are metadata files for 4.x apps and 5.x apps.

Here is the metadata for the 5.x apps:
```
<?xml version="1.0"?>
<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://<customer_name>.i-sightuat.com">
  <md:SPSSODescriptor AuthnRequestsSigned="false" WantAssertionsSigned="false" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</md:NameIDFormat>
    <md:AssertionConsumerService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://<customer_name>.i-sightuat.com/auth/wsfed" index="1" />
  </md:SPSSODescriptor>
</md:EntityDescriptor>
```


### [^5.4.x App] Note
The sso configuration is already implemented in the app. Only the environment variables need to be set.

### [5.4.x App] SERVER_ENVARS in Jenkins

project_name
- This is the project name from the url

identity_provider
- Find the `SingleSignOnService` tag and copy the `location` variable which should be a URL. This should always be the one with `HTTP-Redirect` in the `binding` tag.

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

### Changing binding variable to userID
- get customer to attempt login in order to get the variable name tied to the userID they are sending by looking up the profile object in the server logs.
- In Jenkins, add the following two envars:
```
WSFED_IDENTIFIER=<variable_from_logs>
WSFED_IDENTIFIER_MAPPING=nick
```

### Checking Logs
- log into the database box
- run `docker service ls` and take note of the service-worker boxes
- run `docker service logs <container_id>` on each box and review the logs to see if anything stands out as an issue


### References

- [local testing](https://i-sight.atlassian.net/wiki/spaces/DKB/pages/696025112/Testing+SSO+Locally)
- [5.x SSO development guide](https://i-sight.atlassian.net/wiki/spaces/DKB/pages/696057919/SSO+Development+Guide)
- [atlassian documentation](https://i-sight.atlassian.net/wiki/spaces/DTG/pages/108989269/SSO+Documentation)
- [Sample SSO config](https://github.com/i-Sight/config_umd_v5/commit/55435b5d2d17f712625f2f1fe2f4e4b5676e7992)
- [Nnamdi's notes](https://github.com/CEXNIbe/ReadMe/wiki/SSO-Setup)


***
[Table of Contents](../README.md)