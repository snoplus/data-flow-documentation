---
layout: default
title: Proxies
parent: Grid
grand_parent: SNO+ data-flow manual
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

