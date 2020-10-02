---
layout: default
title: calculate_request_statistics.py
nav_exclude: true
---

## calculate_request_statistics.py


This script is used to summarize the final CPU Usage and the final Storage requirements after a request has been completed. It uses an exactly file to find the jobs that ran and adds all the CPU seconds and the bytes together

```bash
python calculate_request_statistics.py [exactly file] -c [config]
usage: Add up important job statistics from a previous request.
       [-h] [-c CONFIG] exactlyfile

#positional arguments:
#  exactlyfile  Text file containing module, run number, pass number
#optional arguments:
#  -h, --help   show this help message and exit
#  -c CONFIG    CouchDB configuration file
```

Here is an example of the output: 

```bash
The total time was: 737904 CPU Seconds or: 0.00 CPU Years.
Missing run time information for  0  files
The total size of all files is: 47109470059 bytes or:  47.11 GB
```

 For each line in an exactly file, the job information is pulled and the runtime is collected. Then the data doc information is pulled and the storage amount is computed. Also reported are the number of files that are missing a runtime. This can occur from rerunning a completed job or if there are issues with reporting a job's status during processing. 
 
