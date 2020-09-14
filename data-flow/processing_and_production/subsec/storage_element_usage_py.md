---
layout: default
title: storage_element_usage.py
nav_exclude: true
---

## storage_element_usage.py

This script runs the 'lcg-infosites space' command, parses the output and uploads the storage element info to the processing database on couchDB. The script has been put into the crontab to be executed (via an sh script) once per day. The database document which the script creates is given a name which includes a date stamp (see the default argument in the usage example). So, if the script is executed more than once per day, documents will be created with identical names, however each document is also given a 'pass' number and in this case this number will be incremented. The 'storage' website within the data processing pages on snopl.us fetch the database documents created by this script and plot the data.

Example usage follows.
```bash
python bin/storage_element_usage.py [-h]
                                    [--testing-nodb]
                                    -c config
                                    [entry_name]
                                    
Required arguments:
  config               Specify path to config file

optional arguments:
  -h, --help           Show this help message and exit
  entry_name           Specified name for the database document. Default is "se_info_YYYY-MM-DD"
  --testing-nodb       Generates a JSON document but does not upload it to couchDB
```
