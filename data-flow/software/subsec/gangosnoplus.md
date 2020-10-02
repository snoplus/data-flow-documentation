---
layout: default
title: GangaSNOplus
parent: Software
grand_parent: SNO+ Data-Flow Manual
nav_order: 2
---

# GangaSNOplus

Ganga is the software used for job management by the SNO+ production. It is hosted on [GitHub](https://github.com/ganga-devs). The repository is public and SNO+ production developers are expected to submit issues and pull requests with updates where appropriate. 

GangaSNOplus is a SNO+ specific plugin for ganga that is used for production & processing jobs, user jobs, file handlers and a specific backend to handle batch submission with Grid storage (called WestGrid, but to be used on any batch system that the production software submits to). There is an additional backend called Illume that allows the SNO+ interface to the Condor batch system, which is surprisingly different from that of other batch systems.

The GangaSNOplus folder should be separated from data-flow (e.g. made into a git submodule) so that it can also be added to the main ganga distribution. Currently there is just an old set of software within ganga/python/GangaSNOplus, with the up-to-date version in data-flow/gasp/GangaSNOplus. 

The plugin directory structure is organised as follows: 
* GangaSNOplus/Lib/Applications: application modules (interfaces for submission of RAT specific jobs) 
* GangaSNOplus/Lib/Backends: submission backends for SNO+ (currently WestGrid and Illume) 
* GangaSNOplus/Lib/Files: modules to handle input / output files 
* GangaSNOplus/Lib/RTHandlers: Run-time handlers map settings in applications to backends (hidden from users) 
* GangaSNOplus/Lib/Splitters: splitters to e.g. run applications over multiple datasets (by splitting into multiple jobs), note that there are also application specific splitters within the Applications folder. 
* GangaSNOplus/Lib/Utils: utilities used by many of the above modules.
* GangaSNOplus/GASP.ini: a default initialisation file that should be set as the GANGA_CONFIG_PATH environment variable, this sets up defaults required by the GangaSNOplus modules.

### RATProd

RATProd is the application used for all production and processing jobs. There is a corresponding RTRATProd module that contains RTHandlers for submission to the various backends. The ratProdRunner.py script runs on the backend, which is able to run either a rat job or a script (having first setup an environment that includes SNO+ software). If running a RAT job, once it is complete the script will check output data (using a list of expected files set at submission time) to verify their contents; this information is passed back to the submission client via the job output sandbox. Specified output files are then uploaded to storage, with a local grid SE used and RAL as a failover. 
