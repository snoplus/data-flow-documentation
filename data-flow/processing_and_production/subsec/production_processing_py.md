---
layout: default
title: production_processing.py
nav_exclude: true
---

## production_processing.py

This script behaves in nominally the same way as offline_processing.py with the notable exception that it does not check for the existence of real data. This is useful if we want to (re)process monte-carlo outputs, as reprocessing is a natural function of a processing module.

Currently this script does not implement any sort of 'default' processing, but could in principle. For now it is expected that a specific module to run can be specified with –nearline in combination with other flags.

This script can be used to submit run-by-run processing that does not require a number of events, and could be expanded to do more if production_submit is too cumbersome.

Example usage follows.
```bash
usage: production_processing.py [-h]
                                (-R START_RUN END_RUN | -1 RUN [RUN ...] | -L FILE)
                                [-N MODULES] [-o FILE] [-s] [-f]
                                [--force-prereq-pass MODULE PASSNO]
                                [--testing-nocal] [--testing-nodb]
                                config version

Submits processing modules to the production database.

positional arguments:
  config                Specify path to config file
  version               Provide a RAT version

optional arguments:
  -h, --help            show this help message and exit
  -R START_RUN END_RUN, --reprocess-range START_RUN END_RUN
                        Submit jobs for a range of runs (inclusive).
  -1 RUN [RUN ...], --one-shot RUN [RUN ...]
                        Submit jobs for a specific list of runs.
  -L FILE, --runlist FILE
                        Submit jobs for a specific list of runs in the file
                        specified. Format is run numbers separated by
                        newlines.
  -N MODULES, --nearline MODULES
                        Run a specific (set of) module(s) instead of the
                        default list.
  -o FILE, --outlist FILE
                        File to append the created passes to. Useful to track
                        which passes are created for reprocessing runlists
  -s, --skip-ratdb-check
                        Do not check for ratdb tables before submitting runs
                        (useful when producing tables)
  -f, --find-prereq     Use the most recent available prereq instead of
                        requiring it be part of the submission
  --force-prereq-pass MODULE PASSNO
                        Force a prereq to use a particular pass instead
  --testing-nocal       Test mode, force ignore of calibration locks
  --testing-nodb        Test mode, no database updates
```
