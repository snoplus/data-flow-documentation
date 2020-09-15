---
layout: default
title: refresh_guids_by_version.py 
nav_exclude: true
---

## refresh_guids_by_version.py 

This script is used to check the guid listed in the database against the one on the grid. 

```bash
python validation/refresh_guids_by_version.py -c [config] -m [module] -v [version] -f [fileType] -o [list of corrected documents] 
usage: Check the Guids in the database against those in the grid. If the database is incorrect, replace the old guid with the correct guid.
       [-h] [-c CONFIG] [-v RATV] [-m MODULE] [-f FILETYPE] [--start RUNSTART]
       [--end RUNEND] [--dryRun] [-o OUTTEXT]

# Required arguments:
#  -c CONFIG         CouchDB configuration file
#  -v RATV           Rat version to be checked, default  6.3.5
#  -m MODULE         Requested module to check, default Analysis
#  -f FILETYPE       File types to check [ntuple], [ratds]
# Optional arguments:
#  -h, --help        show this help message and exit
#  --start RUNSTART  First run to check, default 0
#  --end RUNEND      Last run to check, default 999999
#  --dryRun          Dry run, produces an output of files that will be corrected.
#  -o OUTTEXT        Output file [guidsCorrected.txt]
```

A utility to check the guids between the database and the registered location on the grid. If an inconsistency is found, it fixes the database to match the grid guid. This version of refresh guids is designed to be run as a cronjob over the current version of RAT. This script is surpassed by [validate_data_documents.py](./validate_data_documents_py.md). 
