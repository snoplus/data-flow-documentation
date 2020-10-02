---
layout: default
title: set_grid_permissions.py
nav_exclude: true
---

## set_grid_permissions.py

This script is used to scan all files and directories recursively on the lfn from a specified head directory. Each file is checked to ensure that the permissions are set to 664 and each directory is checked to ensure that permissions are set to 775. This is done to keep all files on the lfn group writeable so that in the event of a change in the production certificate, write access is preserved in the VO production group. 

```bash
python validation/set_grid_permissions.py [-o BLACKLISTNAME] [-i WHITELISTNAME] [-I] [-r] directory 
# Required arguments:
#  directory             Head directory to examine.
# Optional arguments:
#  -h, --help            show this help message and exit
#  -o BLACKLISTNAME, --output BLACKLISTNAME Specify the name of the blacklist to output to [blacklist.txt].
#  -i WHITELISTNAME, --input WHITELISTNAME Specify the name of the whitelist of specific directories to check.
#  -I, --ignore          Ignore available blacklist.
#  -r, --reverse         Reverse iterate through files and directories.
```
