---
layout: default
title: Configuring new clients
nav_exclude: true
---

## Configuring new clients

New production clients will need access to Ganga (via CVMFS) and a dirac_ui if submitting to the Dirac WMS. If a grid UI has been setup then you should already have CVMFS access.

### Software needed
* gridui: Accessible via /cvmfs/grid.cern.ch
* ganga: Accessible via /cvmfs/ganga.cern.ch
* data-flow: Accessible via git hub
  * Needs pip install to generate libraries
  * Can also scp the data-flow/lib director from Cedar
* data-flow/gasp/sites/[sitename].py: This will define the qualities of the site, including the name, the number of jobs and the RAT versions.

### Configure files needed
* /.gangarc: Configure to your local batch system and directory structure be sure RUNTIME_PATH points to your ganga install.
* /.gangasnoplus: Contains ratdb passwords. May be deprecated.
* data-flow/gasp/config/[sitename].cfg: Make sure this is passed to gasp_client. This contains information about the passwords to access databases.
* data-flow/gasp/sites/[sitename].py: This will define the qualities of the site, including the name, the number of jobs and the RAT versions.

### Environment
A few directories need to be accessible from all nodes:
* ganga_jobs: Path set in postexecute and preexecute in .gangarc.
* gangadir: Ganga information directories, the path is set in .gangarc.
* $TMPDIR: Set in an environment file.
* Grid Certificate: Need a valid grid certificate referenced by $X509_USER_PROXY.
* data-flow/env.sh: May need tweaks to match your environment (i.e. cvmfs not mounted).

### Cron jobs
Typically need two cron jobs. First, clear out anything older than a week in $TMPDIR once a day. This can be done with a command like `find /home/snoprod/tmp/ -mtime +7 -delete > /dev/null` Second, the grid certificates need to be refreshed every hour. See the instructions [here]() or look at what is on Cedar as an active example.

### Screen Sessions
Basically the screen sessions just need loops both checking and submitting jobs. Checking can be done at any time, but submitting can only happen at one site. The script [enqueue_command.py](./enqueue_commands_py.md) synchronizes submission. For an active example, refer to the setup on Cedar.

### Dirac
Dirac is used to interface with the Grid. This provides submission and monitoring tools. To setup the dirac_ui first follow instructions [here](https://www.gridpp.ac.uk/wiki/Quick_Guide_to_Dirac).
```bash
# You will need a script checks dirac proxy validity and reminds operators to renew proxies (the one here
# will have a lifetime of 1000 hours)
dirac-proxy-init -g snoplus.snolab.ca_production -v 1000:00 -M -u $HOME/grid/proxy/dirac_production_proxy
dirac-proxy-init -g snoplus.snolab.ca_user -v 1000:00 -M -u $HOME/grid/proxy/dirac_user_proxy

export X509_USER_PROXY=$HOME/grid/proxy/dirac_production_proxy
# This environment file will be used later by Ganga for job submission
env > /path/to/dirac_production_env

export X509_USER_PROXY=$HOME/grid/proxy/dirac_user_proxy
# This environment file will be used later by Ganga for job submission
env > /path/to/dirac_user_env
```

### Batch
Batch systems directly work with a head submission node i.e. PBS, SLURM, CONDOR, etc. If you are using a batch system, you simply need to create a normal voms proxy and ensure that your ganga is set up for that backend.

### Ganga
If ganga has not been run before, you will need to run it for the first time:
```bash
cd data-flow
source env.sh
ganga
```
This will prompt you to create a new .gangarc, say yes!

Now you have a .gangarc, you first need to edit it such that e.g. Batch submission is setup to correctly submit jobs or Dirac submission knows where your environment file is located. For batch submission this may involve additional editing:
```bash
[Configuration]
Batch = SGE
[SGE]
submit_res_pattern = Your job (?P<id>\d+) (.+) has been submitted
[defaults_SGE]
queue = snoplus
```
while for Dirac it will probably involve:
```bash
[Configuration]
RUNTIME_PATH = GangaDirac
[DIRAC]
DiracEnvFile = $HOME/dirac_ui/dirac_env_file
```
You can see an example of a batch submission on Cedar and a Dirac submission on Liverpool.

Most locations will typically have a .gangarc_production and a .gangarc_processing that point to two separate gangadirs. This is a way of distinguishing between production and processing jobs which usually operate on slightly different timescales.