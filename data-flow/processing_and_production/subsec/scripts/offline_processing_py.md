---
layout: default
title: offline_processing.py
nav_exclude: true
---

## offline_processing.py

This script is used to add jobs to the processing database and is intended for continuous running during data taking. The checks the data-flow database and, once a run is complete (a new run is sensed), jobs are added to the data-processing database depending on the run type as stored in RATDB RUN table.

Example usage (for now, recommended via a screen session):
```bash
usage: offline_processing.py [-h]
                             (-r RUN | -b START_RUN END_RUN | -R START_RUN END_RUN | -A RUN | -1 RUN [RUN ...] | -L FILE)
                             [-t RATDB_TAG] [-N MODULES] [-o FILE] [-s] [-f]
                             [--force-prereq-pass MODULE PASSNO]
                             [--testing-nocal] [--testing-nodb]
                             config version filetype

Submits reprocessing jobs, and loops to sense new runs and submit jobs
automatically.

positional arguments:
  config                Specify path to config file
  version               Provide a RAT version
  filetype              L1 or L2 input files?

optional arguments:
  -h, --help            show this help message and exit
  -r RUN, --start-run RUN
                        Starts a processing loop at the last run greater than
                        this argument not yet processed.
  -b START_RUN END_RUN, --block-run START_RUN END_RUN
                        Starts a processing loop over a block of runs without
                        advancing past the end.
  -R START_RUN END_RUN, --reprocess-range START_RUN END_RUN
                        Reprocesses a range of runs (inclusive). This mode
                        does not loop.
  -A RUN, --reprocess-after RUN
                        Reprocesses all runs after the specified run
                        (inclusive). This mode does not loop.
  -1 RUN [RUN ...], --one-shot RUN [RUN ...]
                        Reprocesses a specific list of runs given after this
                        arugment. This mode does not loop.
  -L FILE, --runlist FILE
                        Reprocesses a specific list of runs listed in the file
                        specified. Format is run numbers separated by
                        newlines. This mode does not loop.
  -t RATDB_TAG, --ratdb-tag RATDB_TAG
                        Use a tagged RATDB state instead of the live version.
  -N MODULES, --nearline MODULES
                        Run a specific (set of) module(s) instead of the
                        default list. Make sure you know what you're doing!
  -o FILE, --outlist FILE
                        File to append the created passes to. Useful to track
                        which passes are created for reprocessing runlists
  -s, --skip-ratdb-check
                        Do not check for ratdb tables before submitting runs
                        (useful when producing tables)
  -f, --find-prereq     Find the prerequisite. Required to run a module without
                        also running the prerequisite in this submission. 
                        Optionally takes one parameter (ratv) to find a prerequisite with a different ratv.
  --force-prereq-pass MODULE PASSNO
                        Force a prereq to use a particular pass instead
  --testing-nocal       Test mode, force ignore of calibration locks
  --testing-nodb        Test mode, no database updates
  ```
  This script has two main running modes: offline processing of detector data, reprocessing of detector data.

Offline processing is accomplished with the –start-run (-r) or –block-run (-b) argument which senses runs that have not yet been considered by this script after or between the run(s) in the argument. Once a new run is identified, its job documents are created according to the processing information available for the specified RAT version. This running mode loops forever sensing new runs all the while.

Reprocessing is accomplished with either the –runlist (-L), –reprocess-range (-R), –reprocess-after (-A), or –one-shot (-1) arguments. These take a filename containing runs separated by newlines, a range, a start run, or a list of runs respectively, and create a new pass for each using the specified RAT version. These modes do not loop, as there is no sensing of runs to be done.

Particular modules can be run instead of the default modules chosen by runtype. To do this, specify those modules with –nearline (-N).

The –outlist (-o) argument allows one to specify a file where the newly created jobs will be stored. This is especially useful when reprocessing to know which passes for each run (and module) are created. The processing_list program in rat-tools/GridTools can take this as an input file with its –exactly argument.

If a major reprocessing needs to occur, there is also the option of processing against a frozen RATDB database.

Currently, if a calibration lock is detected or set by the script (because a PCA/ECA calibration run type is detected) then the script will process no later runs until the calibration lock is released and the validto field set for that calibration type (this is the responsibility of the PMT calibrations group).
