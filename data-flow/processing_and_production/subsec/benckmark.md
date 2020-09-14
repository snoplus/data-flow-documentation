---
layout: default
title: Benckmarking
nav_exclude: true
---

## Benchmarking production files

Benchmarking should be run for new macros that are to be run in production, or for all production macros if processing times have changed (e.g. light yield updates). SNO+ benchmarking code has a user interface on [beta.snopl.us](beta.snopl.us). On the benchmarking request webpage you can run a tagged version of rat, or any version of rat on github via the commit hash.

To update the tagged rat versions available you need to update the couchdb database on [liverpool](http://couchdb.ph.liv.ac.uk:5984/_utils/database.html?benchmarking/_design%2Fbenchmark%2F_view%2Frat_versions). To add a new tagged version, click on any rat version to view the [document](http://couchdb.ph.liv.ac.uk:5984/_utils/document.html?benchmarking/bc59640e09c0e9ead03c37cf9d000e68) of rat versions using the regular snoplus user and password. Double click on the value associated with the key “versions” and a text box will appear showing a list of RAT versions as strings (i.e. “6.16.6”). Add a new element to the list with the an updated RAT version and click the green checkmark next to the text box. Finally click the “Save Document” button in the upper left hand corner of the screen.

Benchmarking runs via a screen session on Cedar. You may need to ensure the benchmarking job submission is set up. In order to start a new benchmarking session, perform the following steps:
```bash
$ cd data-flow/bench
$ source ~/benchmarking/env.sh
$ bin/bench_submit --looptime 900 config/cedar.cfg $HOME/benchmarking/fnctl_lock
```
The benchmarking results page shows the results searchable by user input key and rat version. The production request page will simply send a structured email to the production group with a user's request. The reprocessing request page will also simply send a structured email to the processing group with a user's request. For more information on how to run benchmarking, see the README on [github](https://github.com/snoplus/data-flow/blob/master/bench/README.md).
