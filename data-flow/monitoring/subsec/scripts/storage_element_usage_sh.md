---
layout: default
title: storage_element_usage.sh
nav_exclude: true
---

## storage_element_usage.sh

This script prepares the correct environment in order to run the python script of the same name.

This script (in conjunction with the python script) retrieve storage information from all sites and upload to couchDB. Designed to be configured with cron so that it can run once a day.

NOTE - The client-side Javascript code on the storage monitoring page for SNO+ will filter out the unnecessary sites when viewing plots.

This script hinges on the lcg-infosites command, which it utilizes to pull storage data at the time of calling. It then parses and uploads it to the couchDB.

NOTE - Due to the nature of lcg-infosites retrieving data at the time of calling, self-healing cannot be implemented in a similar fashion as the CPU usage scripts (but perhaps there are alternatives). This is designed to be configured under cron so that it runs once at a day.

Example usage: 
```bash
To run the configured script:
./storage_element_usage.sh

To run the associated python script on its own:
python storage_element_usage.py -c [config]

# Required arguments for python script:
#  -c [config]: Path to the slurm configuration file (in this case, usually config/processing.cfg)
```
