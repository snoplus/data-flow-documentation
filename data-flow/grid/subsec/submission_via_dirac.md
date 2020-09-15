---
layout: default
title: Submission via Dirac
nav_exclude: true
---

## Submission via Dirac

Jobs may be submitted to Dirac using either command-line tools or using a python API. For either method you will need to have Dirac tools available in your environment paths and a valid dirac proxy: 

```bash
# From the install location of the Dirac UI software
. bashrc
dirac-proxy-init -g snoplus.snolab.ca_user -M
```

To submit a simple job using the command line tool, e.g. create a job file `job.jdl` with: 

```bash
[
JobName = "Simple_Job";
## Replace Executable with script name if required
Executable = "/bin/ls";
Arguments = "-ltr";
StdOutput = "StdOut";
StdError = "StdErr";
OutputSandbox = {"StdOut","StdErr"};
##  Add InputSandbox field if running with exe, script, or input data, e.g.:
# InputSandbox = {"/path/to/script.sh"};
## To add a destination or to ensure the job does not go to a certain site:
# Destination = {};
# BannedSites = {};
]
```

To submit, check, and retrieve job outputs: 

```bash


dirac-wms-job-submit job.jdl
# Submission returns a job ID, check with:
dirac-wms-job-status [job ID]
# Once the status is complete, retrieve output sandbox with:
dirac-wms-job-get-output [job ID]
```

Dirac also provides a web based monitor, assuming you're submitting to the GridPP production Dirac service then this monitor is located [here](https://dirac.gridpp.ac.uk/DIRAC). Note that you may have to add a security exception to see the web page depending on your security settings in your browser. 

The python API is a very useful interface for building a job management system (e.g. Ganga), for submitting a simple job it's likely less useful. To submit a simple job from either an interactive python session or using a python script: 
```bash
from DIRAC import S_OK, S_ERROR, gLogger, exit
from DIRAC.Core.Base import Script

Script.parseCommandLine( ignoreErrors = False )
from DIRAC.Interfaces.API.Job import Job
from DIRAC.Interfaces.API.Dirac import Dirac

dirac = Dirac()

j = Job()
j.setExecutable('echo',arguments='hello')

## To set a Destination or ban a site
# j.setDestination('site')
# j.setBannedSites(['site1', 'site2'])

result = dirac.submit(j)
print 'job ID:', result
```
For more information see the [Dirac manual](https://dirac.readthedocs.io/en/latest/UserGuide/index.html).
 
For a list of sites that the GridPP production Dirac instance supports, see [here](https://goc.egi.eu/portal/index.php?Page_Type=Sites). To see the current and planned downtimes, you can look at sites [here](https://goc.egi.eu/portal/index.php?Page_Type=Downtimes_Overview). 
  
