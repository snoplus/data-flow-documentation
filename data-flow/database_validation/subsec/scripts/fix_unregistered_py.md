---
layout: default
title: fix_unregistered.py
nav_exclude: true
---

## fix_unregistered.py

This script is used to scan all data documents in the unregistered view of dflow and ensure they either become registered or have their "notRegistered" tag removed if their replicas exist on the grid already. This script can differentiate between data-processing and data-production databases by supplying the corresponding configuration file (processing.cfg or production.cfg). 
 
 ```bash
python validation/fix_unregistered.py [-c CONFIG]
 
# Required arguments:
#  -c CONFIG         CouchDB configuration file


# Optional arguments:
#  -h, --help        show this help message and exit
#  --dryRun          Dry run will print out whether or not an update is successful without actually updating it.
#  -m FILENAME       Output text for missing file locations on the grid [missingDocuments-yyyy-mm-dd.txt]
#  --start RUNSTART  First row in view to check, default 0
#  --end RUNEND      Last row in view to check, default 50
```

For data-processing database: A data document is considered to be unregistered if not all replicas are seen on the grid or if it's missing a guid as well. The script will loop through all sub runs in a document and find whether or not it is missing a guid and generate one with an surl that is on the grid already. Once this is established, all locations are able to be registered with their corresponding guid. The script is able to handle a variety of cases, such as an lfn not being able to gfal.listreps(lfn) or if a replica is listed on the grid already and the location still has an unregistered tag in the document. Before a document is updated, the code will check to see that all replica locations for a given sub run match the replicas listed on the grid. If the document cannot be registered with its lfn, the srm is added to the mising documents text file and it is not updated. 
 
For data-production database: A production data document is considered to be unregistered if it has a notRegistered tag, is missing a guid or is missing an lfn. The script will loop over all documents in the unregistered view and register each document from its corresponding lfn. If the document cannot be registered with its lfn, the srm is added to the missing documents text file and it is not updated. 
 
