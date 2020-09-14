---
layout: default
title: retry_fail.py
nav_exclude: true
---

## retry_fail.py
```bash
usage: retry_failed.py [-h] [--ask] [--modules MODULES] [--status STATUS]
                       [--fearless]
                       [--startrun STARTRUN | --runrange RUNRANGE RUNRANGE]
                       config ratv

Resets passes in a failed state to a waiting state to run again

positional arguments:
  config                Configuration file for database access credentials
  ratv                  The version of rat used to select passes

optional arguments:
  -h, --help            show this help message and exit
  --ask                 Prompts for each matching job instead of assuming
                        retry.
  --modules MODULES     Restrict to modules matching the (shell-style) pattern
  --status STATUS       Retry jobs of another status, e.g. partFail [failed]
  --fearless            Do not prompt the user to continue - run without fear!
  --delete_attachments  Remove previous attachments (.err,.out) files attached to the job documents
  --startrun STARTRUN   Apply to all runs after this run
  --runrange RUNRANGE RUNRANGE
                        Restrict to a specific run range
 ```
 This script may be used to rerun jobs that failed due to an intermittent issue and would now be expected to finish successfully. No new passes are created, and existing dependencies on the failed jobs will start once the retried jobs finish. This script can also operate on jobs that are not marked "failed". For example "partFail" or "unsatisfied" jobs may be retried by passing those to the –status option. This may even be used on "running" or "submitted" jobs if the underlying site disappears, but be aware that this may result in the job running at more than one site if the site comes back online. The fearless option will not prompt a user before executing, only use if you know exactly what you are doing! The delete_attachments flag can be enable in order to remove old output and error files from previous submission trials.
