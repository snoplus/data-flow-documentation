---
layout: default
title: File transfers
parent: Grid
grand_parent: SNO+ data-flow manual
nav_order: 6
---

# File transfers

All SNO+ data transfers between grid resources (including from the buffer machine at SNOLAB) use grid-FTS (File Transfer Service) to schedule and optimise transfers; grid-FTS uses grid-FTP (based on FTP) for data transfers. User downloads from grid resources that use the GridTools grabber use lcg-utils (lcg-cp) for downloads; there is an option to use gfal-utils (lcg-utils is no longer maintained), however, gfal does not yet provide the same functionality.

Transfers requests using grid-FTS are submitted to https://lcgfts3.gridpp.rl.ac.uk:8443. SNO+ relies on a single FTS to perform all transfers; a Cern [FTS monitor](https://monit-grafana.cern.ch/login) can be used to inspect [SNO+ transfers](https://monit-grafana.cern.ch/dashboard/db/fts-transfers-30-days?orgId=20&from=now-24h&to=now&var-group_by=vo&var-vo=snoplus.snolab.ca&var-src_country=All&var-dst_country=All&var-src_site=All&var-dst_site=All&var-fts_server=lcgfts3.gridpp.rl.ac.uk&var-bin=$__auto_interval). You must log in with the CERN SSO using snoplus_vosupport@snolab.ca as the username with the standard data processing password. If there are any issues with the page, contact monit-support@cern.ch to open a ticket. The production tools include a python module to run command line tools for transfers; the following is a basic guide to submitting transfers directly

```bash


# To transfer a single file
glite-transfer-submit -s https://lcgfts3.gridpp.rl.ac.uk:8443 [source-SURL] [dest-SURL]

# To transfer a set of data, where the file contains space delimited sources and destinations
glite-transfer-submit -s https://lcgfts3.gridpp.rl.ac.uk:8443 -f [file]

# Many other options, including:
#  -K to add checksumming
#  -m [myproxy_server]: to use a myproxy server for renewal

# The output upon submitting a transfer request will be a transfer id, query it with
glite-transfer-status -s https://lcgfts3.gridpp.rl.ac.uk:8443 [id]
# Use -l option to list status for all files
```

Note that the transfers run by Grid-FTS do not interface with the LFC. Scripts in the data-flow repository will catalogue data in the LFC and the production / processing databases once transfers are complete. If transfers are being conducted by hand then it is vital to also catalogue the data in the LFC.
