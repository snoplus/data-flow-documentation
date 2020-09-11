---
layout: default
title: move_data
nav_exclue: true
---

## move_data
This script is used to submit transfers requests to the FTS to move data between processing / production Grid sites. It is also capable of moving RAW data from one site to another in tandem with a running `offsite_transfers`. Whether processing or production is moved depends on the database configured in the settings file. The script is mainly used to transfer from UK/EU Tier2 sites (which have limited storage) to the RAL Tier1 site. Once the data is moved to the destination SE it is deleted from the source SE. The script maintains a maximum number of transfers and also submits in batches when possible to minimize overhead. The script should be run with a valid production proxy available and a production account that regularly updates and delegates production proxies to the Grid-FTS.
```bash
usage: move_data [-h] [--version VERSION | --label LABEL | --raw]
                 [--multiplicity MULTIPLICITY] [--ftsbatch FTSBATCH]
                 [--alt-prefix ALT_PREFIXES] [--submit | --check]
                 settings source destination

positional arguments:
  settings              DFlow settings file
  source                Hostname of the souce SE
  destination           Hostname of the destination SE

optional arguments:
  -h, --help            show this help message and exit
  --version VERSION     Only transfer data from a specific RAT version
  --label LABEL         Only transfer data with a specific label (slow)
  --raw                 Transfer all raw data to another site (implies
                        --submit and assumes offsite_transfers is running to
                        confirm)
  --multiplicity MULTIPLICITY
                        Number of transfers to maintain at once
  --ftsbatch FTSBATCH   Number of transfers per fts submission
  --alt-prefix ALT_PREFIXES
                        Alternate prefixes of source paths to consder (can
                        specify many, longest takes precedence).
  --submit              Run submission code only
  --check               Run checking code only
```
