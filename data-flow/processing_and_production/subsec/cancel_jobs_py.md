---
layout: default
title: cancel_jobs.py
nav_exclude: true
---

## cancel_jobs.py

This script is used to cancel jobs in the production or processing databases. It is useful e.g. if a specific job module is causing lots of failures. In particular, you can specify a job by its run and pass numbers or even its status. It can be useful to cancel a failed job if you need to retry other failed jobs. You can also choose a run range if there is a particular subset of runs that have incorrect information. There is also an option to cancel by pass, this could be useful for rerunning multiple times on the same version. To run this script:
```bash
usage: cancel_jobs.py [-h]  [--passes PASSES] [--runrange RUNRANGE RUNRANGE]
                      [--status STATUS]
                      (--module MODULE | --wildcard WILDCARD)
                      [--incremental]
                      config ratv

Cancel jobs submitted to the GASP queue.

positional arguments:
  config                GASP configuration file.
  ratv                  RAT version of the jobs to cancel

optional arguments:
  -h, --help            show this help message and exit
  --passes PASSES, -p PASSES
                        Reset jobs of a specific pass [default: cancel all
                        passes of selected version]
  --runrange RUNRANGE RUNRANGE
                        Restrict to a specific run range (exclusive)
  --runlist, -L runlist.txt 
                        Restrict to the provided run list file (newline-delimited list of runs to cancel)
  --status STATUS, -s STATUS
                        Reset jobs of a specific status [default:
                        waiting,new,submitted,running]
  --module MODULE, -m MODULE
                        Reset a specific module
  --wildcard WILDCARD, -w WILDCARD
                        Select modules by name pattern (glob style)
  --incremental, -i     Update each job when changed instead of a bulk update
                        at the end

  The run list and run range options are mutually exclusive, so you can only choose one option to submit.

```
