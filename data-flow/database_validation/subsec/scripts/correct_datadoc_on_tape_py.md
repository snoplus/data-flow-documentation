---
layout: default
title: correct_datadoc_on_tape.py
nav_exclude: true
---

## correct_datadoc_on_tape.py

This script is used to correct the location of jobs in their data documents when they have been moved to tape or moved back from tape. Given the list of SRMs, the script will modify their corresponding data documents. When the job has been moved to tape, its location will be changed to `sfu-tape`

```bash
usage: correct_datadoc_on_tape.py [-h] [-c CONFIG] [-f SRM_FILE] [--move-back]
                                  [-s SKIP_MODULES] [--ntuple]

optional arguments:
  -h, --help       show this help message and exit
  -c CONFIG        config file
  -f SRM_FILE      the file of SRMS that has been transferred to or back from
                   tape
  --move-back      modifying data documents for data moving back from tape,
                   default is move-to
  -s SKIP_MODULES  The module to skip when updating documents
  --ntuple         skip ntuple files when updating data documents
```

Example usage:
`python correct_datadoc_on_tape.py -c ../config/processing.cfg -f to-tape.log --move-back`
