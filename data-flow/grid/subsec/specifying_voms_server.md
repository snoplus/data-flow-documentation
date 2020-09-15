---
layout: default
title: Specifying VOMS Server
parent: Grid
grand_parent: SNO+ data-flow manual
nav_order: 3
---

# Specifying VOMS Server

Each grid environment is setup a bit differently than all the others. In many cases these differences are insignificant, and in all cases the sysadmin for the machine can make changes to fix issues. Sometimes, though, the different main grid environment sources (Cern and OSG) disagree on what is correct for a critical component. The VOMS server information is a good example.

Each grid environment must specify at least one VOMS server for each VO. Along with this, the DN of the VOMS server's certificate is specified to keep the system secure. When creating a proxy certificate, voms-proxy-init will check the VOMS servers listed for your VO and contact one of them at random to negotiate VOMS permissions. The contacted server issues your VO permissions and signs them with its certificate. The end result is that any grid service you then interact with must be aware of the VOMS server you contacted, otherwise YOUR proxy certificate will be rejected if you require VO permissions.

As of late 2017, CERN lists three VOMS servers for the SNO+ VO and OSG lists only one: voms.gridpp.ac.uk Fortunately voms.gridpp.ac.uk is on both lists, so if you were using a CERN grid environment and created your proxy certificate there's a 1/3 chance you could talk to OSG resources and would be able to communicate with all CERN resources. All OSG originating proxy certificates will be able to talk to all services. Notably, all storage elements are CERN except for Fermilab, so this issue crops up when downloading data.

There is, however, a method of overriding the grid environment's VOMS specification to our own. As long as the VOMS server we specifiy is listed on all grid-environments VOMS configuration, this will allow us to generate fully working proxy certificates each time. Simply create a file - call it /vomses though the name is unimportant - containing the following line: 
```bash
"snoplus.snolab.ca" "voms.gridpp.ac.uk" "15503" "/C=UK/O=eScience/OU=Manchester/L=HEP/CN=voms.gridpp.ac.uk" "snoplus.snolab.ca"
```
And then when creating your proxy certificate, specify this file: 
```bash
voms-proxy-init --vomses ~/vomses --voms snoplus.snolab.ca
```


This should be unnecessary on a machine where you are the sysadmin and can just edit the files under /etc/grid-security/vomses and is unnecessary on a grid environment configured according to the RAT Companion, but useful when using a shared machine with a CERN grid environment. 
