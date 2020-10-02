---
layout: default
title: Checking and Updating GUIDs
nav_exclude: true
---

## Checking and Updating GUIDs

The script `refresh_guids` loops through a group of runs in the database and compares the guids for each file with the registered location on the grid. If there is a discrepancy, the script will update the database with the current guid on the grid. The script will loop through runs beginning with RUNSTART and ending with RUNEND for a particular MODULE and FILETYPE (typically ntuple or ratds) that has been generated with a set rat version (RATV). If the user includes the “dryRun” option, no changes will be made to the database, but a list of files that should be changes will be created. The output file will contain the lfn file name for the guids that are changed in the database, the new (correct) guid and the old (wrong) guid. Additionally, if any files are not registered appropriately on the grid, but have database entries, they will be flagged for further investigation.
```bash
usage: refresh_guids.py [-c CONFIG] [-v RATV] [-m MODULE] [-f FILETYPE] [--start RUNSTART]
       [--end RUNEND] [--dryRun] [-o OUTTEXT]

optional arguments:
  -h, --help        show this help message and exit
  -c CONFIG         CouchDB configuration file
  -v RATV           Rat version to be checked
  -m MODULE         Requested module to check
  -f FILETYPE       File types to check [ntuple], [ratds]
  --start RUNSTART  First run to check
  --end RUNEND      Last run to check
  --dryRun          Dry run, produces an output of files that will be
                    corrected.
  -o OUTTEXT        Output file [guidsCorrected.txt]
  ```
