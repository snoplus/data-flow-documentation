---
layout: default
title: production_summary.py
nav_exclude: true
---

## production_summary.py 

 This script acts as an extension/replacement of the commonly used "GASP pages" which returns job status information. This script allows for module-by-module information for a specific monte-carlo campaign or reprocessing, where the GASP pages only return info for one pair of (module, ratV). In order to do this the script allows for searches based on a label associated with the jobs or with a module name wildcard + ratV combination. Like the GASP pages, production_summary.py will return info in the standard format of 
 * waiting
 * submitted
 * running
 * completed
 * failed
 * canceled
 * deleted
 
 Get a summary (on cedar) for all **production** jobs labelled as "TeLoaded2018a": 
 ```bash
 python monitoring/production_summary.py --label TeLoaded2018a $HOME/data-flow/gasp/config/production.cfg
```

Get a summary (on cedar) for all **production** jobs that have "Solar" in the module name and ratV 6.17.6: 
```bash
python monitoring/production_summary.py --wildcard Solar --version 6.17.6 $HOME/data-flow/gasp/config/production.cfg
```

Get a summary (on cedar) for all **processing** jobs that are labelled as "AmBe2019a": 
```bash
python monitoring/production_summary.py --label AmBe2019a $HOME/data-flow/gasp/config/processing.cfg

usage: production_summary.py [-h] [--version VERSION]
                             (--label LABEL | --wildcard WILDCARD)
                             config

Output job statuses for a given label set or module wildcard.

positional arguments:
  config                Configuration file for database access credentials

optional arguments:
  -h, --help            show this help message and exit
  --version VERSION     RAT version for jobs to summarize
  --label LABEL, -l LABEL
                        Select modules that belong to specific label
  --wildcard WILDCARD, -w WILDCARD
                        Select modules by name pattern (glob style)
```
