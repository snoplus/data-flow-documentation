---
layout: default
title: validate_data_documents.py 
nav_exclude: true
---

This script is used to check the validity of the file related information on the grid. It requires the RAT version, module and file type to be specified as inputs. Example usage: 

```bash
python bin/manage_production_labels -c [config] -m [module] -v [version] -f [fileType] -o [list of corrected documents]
# Required arguments:
#  -c CONFIG         CouchDB configuration file
#  -v RATV           Rat version to be checked, default 6.16.3
#  -m MODULE         Requested module to check, default Analysis
#  -f FILETYPE       File types to check [ntuple, ratds, ratds_blind, blind_ratds, blind_ntuple]
# Optional arguments:
#  -h, --help        show this help message and exit
#  --dryRun          Dry run, produces an output of files that will be corrected.
#  -o OUTTEXT        Output file [documentsCorrected.txt]
#  --start RUNSTART  First run to check, default 0
#  --end RUNEND      Last run to check, default 999999
```

This function will pull the data document associated with a particular module, version, and filetype. For each run it will loop through the subruns, and check the file type, the replicas registered, the guid, the size, and the checksum of the grid (lfc) file against what is in the couchdb database. If any of the information is different, the couchdb document will be updated to match the information on the grid, unless the “dryRun” option is selected. 
