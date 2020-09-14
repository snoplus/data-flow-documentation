---
layout: default
title: unlocks_jobs.py
nav_exclude: true
---

## unlock_jobs.py
```bash
usage: unlock_jobs.py [-h] config site

Unlocks all jobs locked to a particular site

positional arguments:
  config      A configuration file for database access
  site        The identifying name of the site to unlock

optional arguments:
  -h, --help  show this help message and exit
```
This script can remove the lock on job passes if jobs got stuck for some reason. The lock would prevent any other site from submitting this pass in the future were it to remain in place. In practice since `retry_failed` now unlocks jobs this is likely no longer needed, but if a site goes down while jobs are waiting it may be easiest to identify and release its jobs with this script.
