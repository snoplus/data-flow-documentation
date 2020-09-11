---
layout: default
title: buffer1 Screen Sessions
nav_exclude: true
---

## buffer1 Screen Sessions
Two screen sessions exist on buffer1, these are named according to the dflow script that they run and may be reattached like
```bash
screen -R dflow_register
screen -R offsite_transfers
```
Within the screen session, the two scripts should be run as follows:

For the `dflow_register` session:
```bash
while true; do ./dflow_register config/dflow.js; sleep 900; done
```
For the `offsite_transfers` session:
``bash
./offsite_transfers config/dflow.js
```
On buffer1 under the snotflow user  `/scripts/launch_dataflow.sh` will start those screen sessions and also make sure the proxies are setup.

Both scripts run endless loops internally, if either script is not running it is a sign either that the script has crashed or that something has interrupted them (e.g. a reboot on buffer1). `dflow_register` will sometimes crash and need to be restarted. It is run in an infinite bash while loop for stability.
