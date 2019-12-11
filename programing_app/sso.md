## Configure SSO

Single sign-on configuration.

### `server.env` variables

```
# Add the domain of SAML_WSFED_IDENTITY_PROVIDER_URL to the allowed referrers (e.g. https://shbqa.ais.edu/some.path)
ALLOWED_REFERERS=https://<client>.i-sight.com,https://shbqa.ais.edu

SAML_TYPE=saml-adfs-auth
# SingleSignOnService HTTP-Redirect Location
SAML_WSFED_IDENTITY_PROVIDER_URL=<IDENTITY_PROVIDER_FROM_CLIENT_FILE>
SAMLID_USER_MAPPING=nick
# client isight url
SAML_WSFED_REALM=https://<client>.i-sight.com
SAMLID_CASE_INSENSITIVE=true
SAML_WSFED_PROTOCOL=samlp
SAML_IDENTIFIER_MAPPING=http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
# X509Certificate
SAML_WSFED_CERT=<CERTIFICATE_FROM_CLIENT_FILE>

## if you're encrypting, specify env vars below
SAML_WSFED_SIGNING_KEY_KEY_FILE=/etc/cert/i-sightuat_com.key
SAML_WSFED_SIGNING_KEY_CERT_FILE=/etc/cert/star_i-sightuat_com.pem
SAML_WSFED_DECRYPTION_KEY_FILE=/etc/cert/i-sightuat_com.key
```
### Create logs in Containers before testing out SSO
- Docker exe into container: `docker exec -it <container_id> bash`
- console log the `profile` object in `node_modules/isight/lib/api/saml-adfs-auth.js` & `node_modules/isight/lib/api/saml-auth.js`
- Exit container and commit changes: `docker commit <container_id> <new_container_image_name>`
- run `.start-server-container`

### References

- [atlassian documentation](https://i-sight.atlassian.net/wiki/spaces/DTG/pages/108989269/SSO+Documentation)
- [Sample SSO config](https://github.com/i-Sight/config_umd_v5/commit/55435b5d2d17f712625f2f1fe2f4e4b5676e7992)


***
[Table of Contents](../README.md)