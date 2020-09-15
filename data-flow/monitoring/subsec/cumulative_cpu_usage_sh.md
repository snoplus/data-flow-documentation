---
layout: default
title: cumulative_cpu_usage.sh 
nav_exclude: true
---

## cumulative_cpu_usage.sh 

IT IS IMPORTANT THAT THIS SCRIPT BE RUN ON cedar1 AS THE CLIENT-SIDE WEB CODE USES THAT AS AN IDENTIFIER WHEN RETRIEVING THE DATA.
 
This script prepares the correct environment in order to run the python script of the same name.

This script (in conjunction with the python script) retrieve cumulative CPU usage on Cedar and uploads it to CouchDB. Designed to be configured with cron so that it can run once a day. 

During its check, it will compare the number of days elapsed since the most recent year-start (April 4) and compare that to the number of documents in the database view; if the numbers differ, then something has gone wrong (most likely the server was down) then it will re-compute the cumulative total from the beginning and re-populate the database with the new correct totals. Thus, if configured under cron, it is self-healing and should require no user intervention even if a server outage occurs.

The shell script is configured to plug in today's date and yesterday's date to get the usage of the last 24 hours automatically, and as stated above, will check if any issues arise, will fix itself.

Example usage: 
```bash
To run the configured script:
./cumulative_cpu_usage.sh

To run the associated python script on its own:
python cumulative_cpu_usage.py -c [config] -sd [start date] -ed [end date]

# Required arguments for python script:
#  -c [config]: Path to the slurm configuration file (in this case, usually config/slurm_processing.cfg)
#  -sd [start date]: Start date formatted as mm/dd/yy
#  -ed [end date]: End date formatted as mm/dd/yy
```
