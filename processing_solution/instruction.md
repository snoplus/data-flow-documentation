---
layout: default
title: SNO+ processing solution guide
nav_order: 5
has_children: false
---

# SNO+ Processing Solution Guide

This document written is meant to outline any issues that Processing shifters encounter and provide the solution that was performed to fix them.
Please add an entry to this document, if you’ve encountered a problem that does not have a documented solution (or an easily available solution).

Links to Ticketing Services:
ComputeCanada→ Send email to support@computecanada.ca with your issue
GridPP → Navigate to [https://ggus.eu/](https://ggus.eu/) within a browser containing your grid certificate and click on “submit a ticket”

* ## Problem: Where can I find useful data-flow documentation and/or links? 
**Solution:**
  * [A checklist of requirements before starting shifting](https://docs.google.com/document/d/1tPsz1PcYgd4U1zb8nE0SdzgSf1y5EAc6KSWCjEkIaow/edit)
  * [General Technical Setup](https://www.snolab.ca/snoplus/private/DocDB/0050/005007/003/guide.pdf)
  * [Shifter Tutotial](https://www.snolab.ca/snoplus/private/DocDB/0059/005942/001/2019.08.08_dataflow_KEG.pdf)
  * Two guides to interacting with CouchDB can be found at here: [Doc1](https://www.snolab.ca/snoplus/private/DocDB/0049/004937/001/2018-02-22_Land_dataflow_database.pdf), [Doc2](https://www.snolab.ca/snoplus/private/DocDB/0061/006136/002/Interacting_With_CouchDB_For_Data-Flow.pdf)
  
* ## Problem: How do I run processing/production submission or any other data-flow command?
**Solution:**
  The first thing you can do is to check the scripts under **Processing&Production** in **SNO+ Data-flow Manual**, it gives specific desciption of when and how to use this command.
  
  If you are still not sure about the format, a great suggestion is use `history | grep <comman_name>` on cedar. You can see the history of previous command users, try to find the pattern from the history and replace some past arguments with the one you need to use.
  
  Before using the command, you have several things that need to check:
  * Use `source env.sh` in the `data-flow` directory.
  * Make sure you are in ths data-flow/gasp directory, otherwise you may need to change the path to the script.
  
  As a last resort, the slack channel can be used as a fall back for sourcing information from other shifters and group members.
  
* ## Problem: Where do I access the weekly usage plots for the DWG?
**Solution:**
  The plots are automatically generated each week. You can find the latest ones on cedar, in ` ~/monitoring/processing-plots` directory, using command `ls -ltrh`. The 3 plots needed have the following naming format “weekly-usage-X.png”.

* ## Problem: Wrong CouchDB credentials & misformatted .json submission file
  When running the script `production_submit.py`, for instance:
  ```bash
  python bin/production_submit.py ~/temp_Jonathan/jobs/WaterPhysics/Water_Physics_V2_Respin_Set_4.json -t water-physics-v2 -o ~/temp_Diana/Water_Physics_V2_Respin_Set4.exactly
  ```
  The following error occured:
  ```bash
  Authentication required for CouchDB database at  https://couch.snopl.us/data-production
  Username: snoplus
  Password:
  No module for GeneralPhysicsMC_WaterNn_DecayRun
  No module for GeneralPhysicsMC_WaterPn_DecayRun
  No module for GeneralPhysicsMC_WaterPp_DecayRun
  There was incomplete or missing information. Not submitting any jobs.
  ```
  
  **Solution:**
  **Production should ALWAYS be submitted with the data-processing-ug credentials. Use data-processing-ug to be the user name!** If they aren’t submitted this way no jobs will be created. 
  
  When there are incomplete or missing information errors produced with `production_submit.py`, this means that the spelling of the desired module in the .json file does not match the occurrence of that same module in the `production_information_<ratV>.py`.
  
* ## Problem: Proxy does not have a production role error in Ganga.
  DFLOW emails show that proxy does not have a production role. These errors are produced by the `gasp_client.py` code in data-flow and sent to snoplus_vosupport. 
  
  **Solution:**
  Run `voms-proxy-info --all` to check the proxy information. If the timeleft field shows 0, the proxy is not active and must be reset by Jamie Rajewski (jrajewsk@ualberta.ca)

  If the timeleft field is non-zero and the production role error persists, please contact Jamie. 
  
* ## Disk quota exceeded on Cedar
  If you are prompted with a “disk quota exceeded” message on cedar, this likely means that either the `~/snoprod` or project space has exceeded the storage quota.

  **Solution:**
  Run `diskusage_report` to obtain breakdown of allocated space for these directories. If we have indeed reached the limit ping the processing group in slack.
  
* ## Problem: Need to resubmit canceled job, but there are several canceled passes.
  If you need to resubmit a canceled job the usual way is to use the `retry_failed.py` script from gasp. This will however resubmit all canceled passes for specific run / module combination at once with no option to select a specific pass. If you only need to resubmit the last pass, you can follow the solution here.
  
  **Solution:**
  New view in couchdb was made: https://couch.snopl.us/_utils/#/database/data-processing/_design/dproc/_view/job_passes_by_module_status_reduce_lastpassonly
  that lists only the status of the last pass for a particular run/module. A copy of `retry_failed.py` script was created, called `retry_failed_last.py` (in the same folder), that is an exact copy of `retry_failed.py` BUT using the new view therefore only looping through the latest pass for each run/module.

* ## Problem: Can’t connect to cedar1 but cedar5 is working (or vice-versa)
  Check https://status.computecanada.ca/ for any reports on cedar outages/issues
  
  **Solution:**
  Connect to a particular login node by specifying it at the start of the ssh host. For example , you can run 
  ```bash
  ssh snoprod@cedar5.cedar.computecanada.ca 
  ```
  
* ## Problem: I want to stop a site from running a specific rat version
  With multiple sites running processing or production, the desire to control what kind of jobs run on each can be controlled by constraining it to a certain ratV or preventing it from running a certain ratV. 

  **Solution:**
  In the `~/data-flow/gasp/sites/` directory exists the python site files usually in the format `<site>_<backend>_<processing/production>.py`. In each of these will be a dictionary called rat_versions that point to the env.sh for each rat version. Remove/comment out the line pointing to the environment file you wish to ban from running on that specific site. Add the line back to allow that site to run that rat version. 

* 
