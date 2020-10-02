---
layout: default
title: Troubleshooting
parent: Data-flow
grand_parent: SNO+ Data-Flow Manual
nav_order: 5
---

# Troubleshooting

Most failures of the dflow script are related to issues with grid services, for example unhandled error statuses when submitting file transfer requests. The following should always be true (see [services](./setup_services.md)):
* A valid certificate is available to generate grid proxies is loaded on buffer1
* A valid proxy, with a production role, is being used by the scripts
* Proxies are being delegated to the Grid-FTS before old proxies expire (at least every 12 hours)

The snotflow account on buffer1 has been setup with the following functions in the bashrc:
```bash
function grid_user = {
    export X509_USER_PROXY=${HOME}/grid/proxy/user_proxy
    voms-proxy-info -timeleft
}
function grid_production = {
    export X509_USER_PROXY=${HOME}/grid/proxy/user_proxy
    voms-proxy-info -timeleft
}
```
Typing `grid_production` will inform the user whether the existing production proxy (which is used for data-flow scripts) is still valid.

Assuming a valid proxy is available, inspecting the data-flow script logs should provide further information of the type of error encountered. If one proxy must be updated, it is likely that all proxies on all computers used also need updating.

### Expired CA Certificates

On machines managed by SNO+ (Buffer1) the trusted certificates must be up to date to securely communicate with grid services. When the trusted certificates expire, certificates provided by hosts cannot be validated and connections will fail. It may not even be possible to create a proxy certificate. Messages indicating this is your problem varry from program to program and include but are not limited to:
```bash
Certificate validation error: Can not verify the CRL as its issuer's public key is unknown or can not be validated 
Certification path could not be validated.
The peer's certificate with subject's DN [...] was rejected. 
The peer's certificate status is: FAILED 
The following validation errors were found: error at position 0 in chain, problematic certificate subject: [...]
Can not verify the CRL as its issuer's public key is unknown or can not be validated
SSL Connection Error
```
To fix this, the trusted certificates must be updated. Traditionally this is handled by a system package, and usually requires root permissions to update. On a Scientific Linux or CentOS machine with a grid environment installed via yum, the following command will update the trusted certificates:
```bash
sudo yum update ca_*
```
If you notice this issue happening frequently it is possible that the Certificate Revocation Lists (CRLs) on that machine are not being automatically updated. Contact the responsible person for that machine to see about enabling automatic updates for the CRLs.
