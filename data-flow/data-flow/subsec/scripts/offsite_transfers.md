---
layout: default
title: offsite_transfers
nav_exclude: true
---

## offsite_transfers

This script handles replication of raw detector data (L1, L2, LOG) to the grid sites specified in the script itself. First data identified by dflow_register is submitted for transfer from buffer1 to the grid SE designated as the primary site in the script. Then any data that does not have the desired number of replicas is submitted for transfer for any configured sites it does not exist on. The script maintains a maximum number of transfers per data type (L1, L2, LOG) and also submits in batches when possible to minimize overhead.

```bash
usage: offsite_transfers [-h] [--multiplicity MULTIPLICITY]
                         [--ftsbatch FTSBATCH]
                         settings

positional arguments:
  settings              DFlow settings file

optional arguments:
  -h, --help            show this help message and exit
  --multiplicity MULTIPLICITY, -n MULTIPLICITY
                        Number of transfers to maintain for each file type
  --ftsbatch FTSBATCH, -b FTSBATCH
                        Max number of transfers per FTS submission
```
