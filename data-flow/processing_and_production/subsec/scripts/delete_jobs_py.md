---
layout: default
title: delete_jobs.py
nav_exclude: true
---

## delete_jobs.py

This script is to delete datasets from the grid that are no longer needed for analysis. It will:
1. Perform the query and filtering to find matches in the database
2. Delete the files from the storage element
3. Delete the data document from the database
4. Update the job document to reflect the deletion

Because this script is deleting from the grid, it **requires a valid proxy** - ensure you have run `grid_production` prior to running!

An enhancement was added to perform the grid deletion much faster by utilizing concurrency - see the arguments list below for how to effectively use it.

### Logs and Output

The `-o output_file` option was removed, as the new version of this tool generates its own logs for each stage. The logs are as follows:
1. `TO_DELETE_GRID.log` - srm, filesize
2. `TO_DELETE_DATA_DOCS.log` - dataDocID
3. `TO_MODIFY_JOB_DOCS.log` - jobDocID, pass, subrun

In addition to these, there are an additional 3 logs that contain entries that failed to be deleted/modified. These will only be created if there are entries that failed:
1. `FAILED_DELETE_GRID.log` - srm, filesize
2. `FAILED_DELETE_DATA_DOCS.log` - dataDocID
3. `FAILED_MODIFY_JOB_DOCS.log` - jobDocID, pass, subrun

**NOTE** - These 6 logs are **overwritten** when performing a new deletion. In order to keep track of what you just deleted, please move the generated logs somewhere safe prior to running any additional deletions!

Finally, `DELETED_SIZES.log` contains the total deleted size. This value is also printed at the end of the deletion, but it can be easy to lose track of (especially if doing multiple deletions) so it is added to a file.

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
