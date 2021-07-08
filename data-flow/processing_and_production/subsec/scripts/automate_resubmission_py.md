---
layout: default
title: automate_resubmission.py
nav_exclude: true
---

## automate_resubmission.py

This script is used to automate resubmission Processing or Production jobs with certain requirements:
* Jobs without outputID and attachments in job documents
* Jobs with attachments containing some special keywords e.g. ['gfal-copy error: 70', 'DUE TO TIME LIMIT', 'gfal-copy error: 110 (Connection timed out)']. 
```bash
usage: automate_resubmission.py [-h] [--modules MODULES]
                                [--runrange RUNRANGE RUNRANGE]
                                config ratv

positional arguments:
  config                Configuration file for database access credentials
  ratv                  The version of rat used to select passes

optional arguments:
  -h, --help            show this help message and exit
  --modules MODULES     job module
  --runrange RUNRANGE RUNRANGE
                        Restrict to a specific run range
```

### Tips:
The script is set as a cronjob in retry_jobs.sh  in `cron` directory
General Format:
  `python bin/automate_resubmission.py config/processing.cfg 6.18.11`
