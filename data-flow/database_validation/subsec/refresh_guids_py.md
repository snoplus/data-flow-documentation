---
layout: default
title: refresh_guids.py
nav_exclude: true
---

## refresh_guids.py 

This script is used to check the guid listed in the database against the one on the grid. 

```bash
python validation/refresh_guids.py -c [config] -m [module] -v [version] -f [fileType] -o [list of corrected documents] 
usage: Check the Guids in the database against those in the grid. If the database is incorrect, replace the old guid with the correct guid.
       [-h] [-c CONFIG] [-v RATV] [-m MODULE] [-f FILETYPE] [--start RUNSTART]
       [--end RUNEND] [--dryRun] [-o OUTTEXT]

# Required arguments:
#  -c CONFIG         CouchDB configuration file
#  -v RATV           Rat version to be checked, default  6.3.5
#  -m MODULE         Requested module to check, default Analysis
#  -f FILETYPE       File types to check [ntuple], [ratds]
#  --start RUNSTART  First run to check
#  --end RUNEND      Last run to check
# Optional arguments:
#  -h, --help        show this help message and exit
#  --dryRun          Dry run, produces an output of files that will be corrected.
#  -o OUTTEXT        Output file [guidsCorrected.txt]
```

This script is surpassed by [refresh_guids_by_version.py](./refresh_guids_by_version_py.md). 
