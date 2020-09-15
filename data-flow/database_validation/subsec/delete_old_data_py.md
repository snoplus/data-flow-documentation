---
layout: default
title: delete_old_data.py
nav_exclude: true
---

## delete_old_data.py

This script is used to delete old data. This is performed in different steps depending on whether it is processing data, production data or pure data files. For production and processing data: the script queries for job files given the arguments, loops through the passes, loops through the subruns, collects the outputs and updates the job documents - removes outputs and sets the pass as deleted. For pure data only the data IDs are collected.

When the array of data files is collected, the next step is to loop through the files: firstly a check is made to confirm this passes the selection. Then it loops through files and locations, tries to remove the physical files from their respective locations and finally deletes the data document.

An output file is created summarising the process.

There are confirmation Y/N question throughout the process to confirm any file deletion and database changes.

Example usage: 

```bash
python validation/delete_old_data.py -c [config] -m [module] -v [rat_version] --start [start_run] --end [end_run] -type [type] -o [output] -f [fileType]
# Required arguments:
#  -c [config]: CouchDB configuration file
#  -m [module]: Module of the data to be deleted
#  -v [rat_version]: RAT version of the data to be deleted
#  --start [run number]: First run to label
#  --end [run number]: Last run to label (inclusive)
#  -type [type]: Type of data to be deleted: "Processing", "Production" or "Data"
# Optional arguments:
#  -o [output]: name of the output file, set by default to db_val.csv
#  -f [fileType]: the file-type to be deleted, set to ratds by default
```

The type has to be specified as this sets how the job files are collected and what the selection criteria are. Please select one of "Processing", "Production" or "Data". Processing and Production will also update the job documents. 

The selection parameters are:
* rat version, module and file type for Data
* rat version, module, file type and run for Processing and Production 

Example usage for processing: - python delete_old.py -c ../config/slurm_processing.cfg -v 6.2.6 -m TestProc –start 123456 –end 123456 -type Processing

Example usage for production: - python delete_old.py -c ../config/slurm_production.cfg -v 6.2.6 -m TestProd –start 123456 –end 123456 -type Production

Example usage for data: - python delete_old.py -c ../config/slurm_production.cfg -v 6.2.6 -m TestData –start 123456 –end 123456 -type Data 

The code itself is divided into 3 different parts: 
* Loops through the job documents to collect data IDs. Removes outputs and updates the documents. This is skipped for "Data" mode. 
* Loops through the data documents, collecting the files. Removes the data documents.
* Tries to delete the files from grid. 

The process is output to terminal which serves as secondary check. Additionally there is output file created. There are several Y/N checks at different points in the script before any changes are performed. 
