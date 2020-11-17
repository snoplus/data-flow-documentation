---
layout: default
title: delete_jobs.py
nav_exclude: true
---

## delete_jobs.py

This script is to delete datasets from the grid that are no longer needed for analysis. It will delete the files from the storage element, remove the lfc, up date the job document to reflect the deletion and delete the data document from the database. 

An enhancement was added to perform the grid deletion much faster by utilizing concurrency - see the arguments list below for how to effectively use it.

You can give it a list in a file that contains the module and rat version separated by a comma, or you can enter the module and rat version in line. You can only enter a list or a module/rat version not both. An example of a list file:
```bash
Processing,6.16.1
Analysis10,6.5.5
```
Either way you need to supply a config file to supply the database and password.

See the list of arguments below for a breakdown of options to pass.

```bash
python bin/delete_jobs.py -l [list] -c [config]
python bin/delete_jobs.py -m [module] -v [ratv] -c [config]
# Required arguments:
# -l [list]: List of modules and ratv to be deleted [TeLoadedBi214, 4.5.0]
# -m [module]: Module of the production that needs to be deleted
# -v [ratv]: Rat version of the module to be deleted
# -c [config]: A configuration file that contains the database details
# Optional argument
# --processes [process_count]: Enter a value higher than one to create multiple processes to perform SRM deletion concurrently. Greatly speeds up large deletions. For best results, enter a number less than the total number of logical cores in your machine (for example, a machine with 4 cores/8 threads, enter 6 or 7). Default is 1
# --debug: Enables verbose debug output (shows individual affected SRMs, document IDs, etc)
# -t [fileType]: the file type that is to be deleted. The default is to delete the ratds, ntuple, and soc files.
# -s [status]: the status of the jobs to be deleted. Default is 'canceled'
# -p [passes]: the pass number of the jobs to be deleted. Default is all passes
# -d [db_tag]: the db_tag to be deleted. Default is all tags
# --runrange [start end]: the run range for the jobs to delete. Default is all runs
```
