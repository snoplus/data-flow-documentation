---
layout: default
title: Proxies
parent: Grid
grand_parent: SNO+ Data-Flow Manual
nav_order: 2
---

# Proxies

Production accounts need to have valid grid proxies, and VOMS proxies are short-lived and require password authentication. To get around this, we generate long term proxies (myproxy) that can be used to renew the short term VOMS proxies. Note that a production Dirac proxy is also required if you are running grid jobs, these act similarly to myproxies in that they can be created as valid for a long time (e.g. 30 days), checks should be implemented to send warnings when both myproxy and dirac proxies need renewing. 

### For DIRAC jobs:
Establish a Dirac proxy with: 
```bash
# First source the dirac environment, e.g cd dirac_ui && . bashrc

# Generate a proxies that are uploaded to the Dirac server
dirac-proxy-init -g snoplus.snolab.ca_user -v 1000:00 
dirac-proxy-init -g snoplus.snolab.ca_production -v 1000:00 

# Generaate proxies that are stored locally for job submission
dirac-proxy-init -g snoplus.snolab.ca_user -v 1000:00 -M -u $HOME/grid/proxy/dirac_user_proxy
dirac-proxy-init -g snoplus.snolab.ca_production -v 1000:00 -M -u $HOME/grid/proxy/dirac_production_proxy
```

### For All Systems:
Establish a long term proxy with: 
```bash
myproxy-init -l snoprod -k snoprod -t 36 -c 8760
```
You will be prompted for a password which should be different to any other password you use and can be simple and stored in a plaintext file (read only by you!). On Cedar, this password is kept in /cron/refresh/snoprod.txt. On each machine that you wish to renew short proxies, you will first need a valid short proxy before retrieving the delegation (so e.g. first setup a short proxy, then continue to renew using a cron job).

This script will set up your environment to be ready for a proxy renewal: 
```bash
# Setup script to create the right environment for a proxy

#!/bin/bash

export MYPROXY_SERVER=myproxy.cern.ch
export LFC_HOST=lfc.gridpp.rl.ac.uk
export LCG_GFAL_INFOSYS=lcg-bdii.cern.ch:2170
#export LCG_GFAL_INFOSYS=lcgbdii.gridpp.rl.ac.uk:2170

export GRID_USER=$HOME/grid/user_proxy
export GRID_PRODUCTION=$HOME/grid/production_proxy
export GRID_PRODUCTION_ROLE=$HOME/grid/production_proxy\:snoplus.snolab.ca_production

function grid_user {
    export X509_USER_PROXY=$GRID_USER
    voms-proxy-info -timeleft
}
function grid_production {
    export X509_USER_PROXY=$GRID_PRODUCTION
    voms-proxy-info -timeleft
}
```
This script will renew a proxy: 
```bash
# Refresh script running as a cron job every hour

#!/bin/bash -l

source $HOME/grid/cern_env.sh
# grid user is a function defined in ~/grid/proxies.sh on cedar (see above)
grid_user

myproxy-get-delegation -l snoprod -k snoprod -S < $HOME/cron/refresh/snoprod.txt

voms-proxy-init --vomses ~/grid/vomses --valid 12:00 --voms snoplus.snolab.ca:/snoplus.snolab.ca/Role=production -noregen -out $GRID_PRODUCTION
voms-proxy-init --vomses ~/grid/vomses --valid 12:00 --voms snoplus.snolab.ca -noregen -out $GRID_USER

cp $GRID_PRODUCTION $GRID_PRODUCTION_ROLE
```

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

# Current VOMS Information

The up-to-date VOMS information can be found [here](https://voms.gridpp.ac.uk:8443/voms/snoplus.snolab.ca/configuration/configuration.action). Note that this page may require a valid user certificate loaded in your browser to access.
