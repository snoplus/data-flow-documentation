---
layout: default
title: gasp_client.py
nav_exclude: true
---

## gasp_client.py

The script `gasp_client.py` is used for all submission of production and processing jobs. The script runs via a cron job, however, it may also be useful to run from the command line when e.g. debugging or commissioning a new site. Example usage:
```bash
# Run with the default $HOME/.gangarc:
ganga bin/gasp_client.py -c config/site.cfg [queuetype]
# Run with a specific ganga profile (useful for production clients, multiple clients on one server etc):
ganga --config=$HOME/.gangarc_production bin/gasp_client.py -c config/site.cfg [queuetype]
# Run monitoring only with 1000 steps in the monitoring loop:
ganga bin/gasp_client.py -c config/site.cfg --mononly -s 1000 [queuetype]
# Run submission only:
ganga bin/gasp_client.py -c config/site.cfg --nomon [queuetype]
# In all cases [queuetype] should be one of:
# --runlevel - submits smaller numbered runs first
# --newfirst - submits higher numbered runs first
# --priority - submits jobs according to their priority (larger first)
# --serial   - submits jobs in the order they were added to the database (fifo)
```
Two configuration files are required to run `gasp_client.py`: the config/[main-config].cfg and another in sites/[site-config].py. Templates for each are available at config/template_config.cfg and sites/template_site.py.

To drain jobs, you should ensure that scripts are running in mononly mode (this ensures that no new jobs are submitted and is useful e.g. when changing certificates on the account). Note that you can also start ganga interactively, kill all jobs (jobs.remove()) and then run in mononly mode if looking for a quick fix if e.g. the server is entering downtime. The jobs will be seen as missing and reset to a waiting state in the database.

The queue type may be changed at any time depending on the desires of the processing group at the moment. In general the processing queues will be –runlevel, with –newfirst as an option during times with large backlogs. Production queues will in general be –priority, however –serial may be used to submit old jobs.

The delete_attachments flag (–delete_attachments) can be enable in order to remove old output and error files from previous submission trials.
