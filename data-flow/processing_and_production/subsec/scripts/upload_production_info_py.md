---
layout: default
title: upload_production_info.py
nav_exclude: true
---

## upload_production_info.py

This script is used to upload prodcution_information files to couchdb. It is useful when you modify existing production_information file. If you would like to generate a production_information file for a new RAT version, you can use `make_production.py` instead.
```bash
usage: upload_production_info.py [-h] [-c CONFIG] [-o OUTPUT]

optional arguments:
  -h, --help  show this help message and exit
  -c CONFIG   CouchDB configuration file (USE slurm_processing.cfg)
  -o OUTPUT   Output filename like production_information_X_Y_Z.py
  ```
  
  Example usage:
  ` python bin/upload_production_info.py -c config/slurm_processing.cfg -o production_information_6_18_9.py`
