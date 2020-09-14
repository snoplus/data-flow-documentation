---
layout: default
title: production_submit.py
nav_exclude: true
---

## production_submit.py

This script is used to add production jobs to a database, note that it should not be used for processing jobs.
```bash
usage: production_submit.py [-h] [-t RATDB_TAG] [-o FILE] 
                            [--database DATABASE] [--server SERVER]
                            [--testing-nodb]

                            jobfile

Submits production jobs.

positional arguments:
  jobfile              JSON document describing jobs to submit

optional arguments:
  -h, --help           show this help message and exit
  --database DATABASE  CouchDB database to use
  --server SERVER      CouchDB server to use
  -o FILE, --outlist FILE
                       File to append the created passes to. Useful to track
                       which passes are created for production runlists
  -t RATDB_TAG, --ratdb-tag RATDB_TAG
                       Use a tagged RATDB state instead of the live version.
  -L FILE, --runlist FILE       
                       Run production over a newline-delimited file list of
                       run numbers
  --testing-nodb       Do not commit changes to DB
  ```
  The script requires preparation of a job description file containing JSON that describes the jobs to be added. There is an option to produce Monte Carlo against a “frozen” RATDB database that should be used for large requests in case someone uploads a new table. There is also an option to have an output list that can be used to download Monte Carlo using the exactly option in the grid tools of rat-tools. The overall structure of the JSON depends on the types of jobs being added to the database. The following should cover most of the expected formats:
  ### Jobs to be run by time:
  ```bash
  {
  "rat_v": "5.3.2",
  "modules" : [
    {"module": "TeLoadedBipo212-pure_RAT5", "t_run":365, "t_format": "day", "n_runs":565},
    {"module": "TeLoadedBipo214-pure_RAT5", "t_run":365, "t_format": "day", "n_runs":12245}
  ]
}
```
### Jobs to be run by number of events, that also include template parameters that need to be set in the macro:
```bash
{
  "rat_v": "5.3.2",
  "modules" : [
    {"module": "WaterPmt_Betagammas", "n_ev":985000000, "ev_per_run":290000, "template": {"radius": 7250}},
    {"module": "WaterPmt_Betagammas", "n_ev":928000000, "ev_per_run":290000, "template": {"radius": 8000}}
  ]
}
```
### Jobs to be run with real run parameters for specific runs, that also specify a priority for the jobs:

*Option 1 - Specify the runs manually in a list within the command*
```bash
{
"rat_v": "6.3.2",
"modules" : [
             {"module": "WaterBi214Run", "n_runs": 1, "run_numbers": [100151,100152], "priority": 100},
             {"module": "WaterBi214_ExwaterRun", "n_runs": 1, "run_numbers":[100151,100152], "priority": 100}
  ]
}
```
*Option 2 - Specify a file containing a runlist during submission:*
```bash
Notice that run_numbers is now removed, and the same runlist in the production_submit command gets supplied to all modules

jobfile.json
{
"rat_v": "6.3.2",
"modules" : [
              {"module": "WaterBi214Run", "n_runs": 1, "priority": 100},
              {"module": "WaterBi214_ExwaterRun", "n_runs": 1, "priority": 100}
  ]
}

runs.txt
100151
100152

Running production_submit.py...
production_submit.py -L runs.txt jobfile.json
```
