---
layout: default
title: storage_site_file_scan.py
nav_exclude: true
---

## storage_site_file_scan.py

This script flags and unregisters the zdabs on certain grid storage site and remove their corresponding CouchDB references. Example usage: 

```bash
python validation/storage_site_file_scan.py -c [config] --start [runStart] --end [runEnd] -f [fileType] -o [output text file] --site [Storage site of interest] --dryRun --verbose

# Required arguments:
#  -c CONFIG         CouchDB configuration file
#  -f FILETYPE       File type to check [L1], [L2]
#  -o OUTTEXT        Output file [files_on_site.txt]
#  --start RUNSTART  First run to check
#  --end RUNEND      Last run to check
#  --site SITE       Storage site to check
# Optional arguments:
#  -h, --help        show this help message and exit
#  --dryRun          Dry run, produces an output of files that will be modified and unregistered.
#  --verbose         Increase output verbosity
```

In the dryRun mode, the script will only flag the files on the storage site of interest without modifying the documents and unregistering the files. The output file will contain the document ID, run number, file type, subfile number, guid, and the srm path to the zdab. Before unregistering files and modify CouchDB documents, you should always dryrun on a small number of runs to see if the output text file contains the relevant information of the runs. After making sure everything looks fine, remember to set up the grid_production proxy before a wet run.
