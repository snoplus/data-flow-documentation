---
layout: default
title: enqueue_command.py
nav_exclude: true
---

## enqueue_command.py

This script solves the problem of running arbitrary commands on separate machines as frequently as possible while ensuring that only one machine is executing at a time.

To do this a couch document (enqueue_leader) specifies which site is allowed to run and its current status. The leader document also specifies a site as the leader which is responsible for choosing the next site to run when the current site is finished. Independently of this document, each site periodically updates a document with the current UTC time as a heartbeat. The sites add the key to their heartbeat document to another document (enqueue_sites) so that other sites are aware. Active sites are all those listed in the sites document that have a valid heartbeat.

If there is no leader, or the leader is no longer active, there is an election. The race that is won by the site that asserts itself as leader first. This is done by updating a couch document, so only the true and unambiguous leader's update will succeed.

Each time the leader site updates its heartbeat, it also checks the status of the running site. If the site reported it is done or the site has become inactive, the next site (alphabetically) is set as the executing site.

The once a site is identified as running, a subprocess is launched to execute the command specified.

Optionally, and rarely in practice, enqueue_command can repeat the specified command in a loop or use alternatively named documents.
```bash
usage: enqueue_command.py [-h] [--heartbeat-delay MIN] [--heartbeat-stale MIN]
                          [--conflict-delay MIN] [--repeat]
                          [--time-format FORMAT] [--leader-doc NAME]
                          [--sites-doc NAME]
                          config sitename [cmdargs [cmdargs ...]]

Uses couchdb to serialize commands across many hosts.

positional arguments:
  config                A configuration file for database access
  sitename              Unique identifier for this site
  cmdargs               The command to enqueue

optional arguments:
  -h, --help            show this help message and exit
  --heartbeat-delay MIN, -d MIN
                        Minutes between heartbeats [10]
  --heartbeat-stale MIN, -s MIN
                        Minutes after heartbeat is stale [30]
  --conflict-delay MIN, -c MIN
                        Time to wait after a conflict [1]
  --repeat, -r          Repeat the command [do not repeat]
  --time-format FORMAT, -T FORMAT
                        Database datetime format [%Y-%m-%d %H:%M:%S]
  --leader-doc NAME, -L NAME
                        Document ID for the leader document [enqueue_leader]
  --sites-doc NAME, -S NAME
                        Document ID for the sites document [enqueue_sites]
```
Currently this script is used to run production submission at all sites. Each site has a version of the following bash script running in a bash loop in a screen session. This bash code only serializes the job submission as job checking can take a very long time (Dirac backends) and need not be exclusive as it does not access the database. ADDSITENAMEHERE takes the place of the sitename. This need not be anything specific, but should be unique. The machine's hostname works well. Be sure the configuration files and sourced scripts are appropriate for the site being setup.
```bash
#!/bin/bash -l
source $HOME/grid/cern_env.sh
grid_production
cd $HOME/data-flow
source env.sh
cd $HOME/data-flow/gasp
# Run the job monitoring (check backend, update local ganga)
# -s is the number of times to check all jobs ("monitoring steps")
# -m is the number of seconds to wait for the steps to finish 
ganga --config=$HOME/.gangarc_production bin/gasp_client.py --priority -c config/production.cfg -m 120 -s 2 --mononly
# Enqueue the job submission (also commits local job status changes to couch database)
python bin/enqueue_command.py  config/production.cfg ADDSITENAMEHERE -- ganga --no-mon --config=$HOME/.gangarc_production bin/gasp_client.py --priority -c config/production.cfg --nomon
```
