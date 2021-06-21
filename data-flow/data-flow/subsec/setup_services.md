---
layout: default
title: Setup & Services
parent: Data-flow
grand_parent: SNO+ Data-Flow Manual
nav_order: 3
---

# Setup & Services

In order for the data-flow scripts to run a number of prerequisites must be met:
* A valid certificate is available to generate grid proxies is loaded on buffer1 (note that this uses the snoprod user with a production role, the password is the typical data-flow password);
* A valid proxy, with a production role, is available and used by the scripts;
* Proxies are being delegated to the Grid-FTS before old proxies expire (at least every 12 hours).

It is the data-flow group's responsibility to ensure that the certificate on buffer1 is updated whenever it is renewed.

The short term proxies used for data uploads are renewed using delegations from a long term proxy (myproxy). It is the data-flow group's responsibility to ensure that a valid long term proxy is available to renew the short term proxy. For more details on setting up a long term proxy, refer to the Grid/Proxies section [*].

Short term proxies are generated using the script $HOME/grid/proxy/refresh/refresh.sh, this is run by a cron job once per hour.

## Host Certificates:

Additionally, buffer1 hosts an FTS endpoint in order to transfer data offsite. This requires access to a Host Certificate and running a gridftp server.

To acquire a host certificate, go [cert.gridcanada.ca](cert.gridcanada.ca). When requesting a certificate, select “Server Request”. Enter “snoplusdata.snolab.ca” for the Host Name. It may be necessary to email Lixin Liu (liu@sfu.edu) and notify Erming Pei (erming.pei@computecanada.ca) at ComputeCanada to ensure the “host/” is not appended to the name. When the certificate is prepared, download the p12 file from your browser. On Mozilla Firefox, this can be done by going to “Settings” then “Privacy and Security” and “View Certificates”. After backing up the certificate as a .p12 file, export the key and certificate:
```bash
openssl pkcs12 -nocerts -in backup.p12 -out hostkey.encrypted.pem
openssl pkcs12 -clcerts -nokeys -in backup.p12 -out hostcert_noText.pem
```
We want to keep the key and certificate in a secure way that doesn't require a password. Export the previous .pem files to create an rsa key and a X.509 certificate:
```bash
openssl x509 -in hostcert_noText.pem -text > hostcert.pem
openssl rsa -in hostkey.encrypted.pem -out hostkey.pem
```
Then correct the permissions of the file, these files need to be owned by the root user:
```bash
chmod 664 hostcert.pem
chmod 600 hostkey.pem
```
Move the hostcert and hostkey files to buffer1 and place them into the /etc/grid-security/ folder. You will need root permission (or ask a friend with root permission) to move the files to that directory. All members of the DAQ group will have root access to buffer1. Finally, restart the FTS server to pick up the new certificate (again, root permission needed):
```bash
sudo /etc/init.d/globus-gridftp-server restart
```

## Current Expiration:

The **user** certificate is under Carsten CN and exists on both cedar and buffer1 under the `~/.globus` directory

The **host** certificate exists only on buffer1 under the `/etc/grid-security/` directory, and was updated on **June 17 2021** set to expire on **July 16 2022**.

## Updating the gridmap-file:
In the event that the production certificate was updated with a new DN (new username), a gridmap-file needs to be updated in order to push files from buffer1 to the FTS. This file exists under `/etc/grid-security/grid-mapfile` on buffer1 and can be automatically updated via:
```bash
edg-mkgridmap group voms://voms.gridpp.ac.uk:8443/voms/snoplus.snolab.ca   .snoplus.snolab.ca > /etc/grid-security/grid-mapfile
```
The file can also be updated by hand.

