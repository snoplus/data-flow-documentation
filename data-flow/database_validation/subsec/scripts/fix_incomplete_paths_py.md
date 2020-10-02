---
layout: default
title: fix_incomplete_paths.py
nav_exclude: true
---

## fix_incomplete_paths.py

This script is used to correct the paths of some production data files on the grid. In some cases, the correct path at the host site (usually RAL - GridPP) has been replaced by INCOMPLETE. 

```bash
python validation/fix_incomplete_paths.py [config] [version] -m [module] -format [fileType] 
usage: fix_incomplete_paths.py [-h] [--ask] [--modules MODULES]
                               [--format FORMAT] [--fearless]
                               [--startrun STARTRUN | --runrange RUNRANGE RUNRANGE]
                               config ratv

#positional arguments:
#  config                Configuration file for database access credentials
#  ratv                  The version of rat used to select passes

#optional arguments:
#  -h, --help                    show this help message and exit
#  --ask                         Prompts for each matching job instead of assuming fix.
#  --modules MODULES             Restrict to modules matching the (shell-style) pattern
#  --format FORMAT               Specify the file format (ntuple, ratds, soc or all)
#  --fearless                    Do not prompt the user to continue - run without fear!
#  --alldocs                     Set the script to run over all docs in the view 
#  --startrun STARTRUN           Apply to all runs after this run
#  --runrange RUNRANGE RUNRANGE  Restrict to a specific run range
#  --docrange DOCSTART DOCEND    Restrict --alldocs to run between a specific range in the view
```

Some documents will have INCOMPLETE in the data file URL like:
`srm://srm-snoplus.gridpp.rl.ac.uk/INCOMPLETE/production/TeLoadedAlphan_Telab_Avin_Av_18o/r600/TeLoadedAlphan_Telab_Avin_Av_18o_r629_s0_p2.ntuple.root`

This script loops over the files specified by the user (RAT version, modules, file formats, run range) and looks for the "INCOMPLETE" substring in the data path of the data documents.
Since so far, all the issues have been seen at RAL (srm-snoplus.gridpp.rl.ac.uk), the "INCOMPLETE" are replaced by "/castor/ads.rl.ac.uk/prod/snoplus/". Those paths are hard coded at the moment (Feb 2019) but this might be changed in cases other sites show this issue.
If a data document is found to be linking a host different than RAL, hepgrid11, susx, or qmul, the script will not change the path of the data file in order to avoid dangerously messing up the database.
Note that the default file format is "all", meaning that the script will loop over all the ntuple, ratds and soc files satisfying the conditions set by the user unless told otherwise.

