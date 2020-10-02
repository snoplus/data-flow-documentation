---
layout: default
title: delete_jobs.py
nav_exclude: true
---

## delete_jobs.py

This script is to delete datasets from the grid that are no longer needed for analysis. It will delete the files from the storage element, remove the lfc, up date the job document to reflect the deletion and delete the data document from the database. You can give it a list in a file that contains the module and rat version separated by a comma, or you can enter the module and rat version in line. You can only enter a list or a module/rat version not both. Either way you need to supply a config file to supply the database and password. You can also specify the file type that you want to delete as well. If no file type is given it will presume you mean to delete all ratds, ntuple and soc files. You can specify the status of the jobs to delete ('canceled' is the default), the pass number (all passes is the default) and the run range (all runs is the default)
```bash
python bin/delete_jobs.py -l [list] -c [config]
python bin/delete_jobs.py -m [module] -v [ratv] -c [config]
# Require arguments:
# -l [list]: List of modules and ratv to be deleted [TeLoadedBi214, 4.5.0]
# -m [module]: Module of the production that needs to be deleted
# -v [ratv]: Rat version of the module to be deleted
# -c [config]: A configuration file that contains the database details
# Optional argument
# -o [outText]: names the output text file that is produced. The default is documentsDeleted.txt
# -t [fileType]: the file type that is to be deleted. The default is to delete the ratds, ntuple, and soc files.
# -s [status]: the status of the jobs to be deleted
# -p [passes]: the pass number of the jobs to be deleted
# --runrange [start end]: the run range for the jobs to delete
```
