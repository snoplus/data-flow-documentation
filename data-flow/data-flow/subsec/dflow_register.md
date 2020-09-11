---
layout: default
title: dflow_register
nav_exclude: true
---

## dflow_register

The script `dflow_register` detects files from the builder and L2 (stored in /raid/data/l1 and /raid/data/l2 respectively) and registers them in a database. The script will also be responsible in the future for making the decision on which L1 files to transfer. Currently 100% of L1 data written to the l1 folder is transferred for water data. However, with the expected increased rate for scintillator, likely only 10% of the L1 files will be saved.

```bash
usage: dflow_register [-h] [-n NAME] [-m MONITOR] -s

positional arguments:
  -s                    Settings file

optional arguments:
  -h, --help            show this help message and exit
  -n NAME, --name NAME  source_id
  -m MONITOR, --monitor MONITOR
                        monitor name
```
