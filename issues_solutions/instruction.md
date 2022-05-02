---
layout: default
title: SNO+ Issues & Solutions Guide
nav_order: 6
has_children: false
---

# SNO+ Processing - Issues & Solutions Guide

This is a compilation of issues and solutions that processing shifters have encountered. Please add an entry to this document if you’ve encountered a problem that does not have a documented solution (or an easily available solution).

Contact info for various support services can be found [here](../communication/list.md).

---

## **Where can I find useful data-flow documentation and/or links?**

**Solution:**
  * [A checklist of requirements before starting shifting](https://docs.google.com/document/d/1tPsz1PcYgd4U1zb8nE0SdzgSf1y5EAc6KSWCjEkIaow/edit)
  * [General Technical Setup](https://www.snolab.ca/snoplus/private/DocDB/0050/005007/003/guide.pdf)
  * [Shifter Tutotial](https://www.snolab.ca/snoplus/private/DocDB/0059/005942/001/2019.08.08_dataflow_KEG.pdf)
  * Two guides to interacting with CouchDB can be found at here: [Doc1](https://www.snolab.ca/snoplus/private/DocDB/0049/004937/001/2018-02-22_Land_dataflow_database.pdf), [Doc2](https://www.snolab.ca/snoplus/private/DocDB/0061/006136/002/Interacting_With_CouchDB_For_Data-Flow.pdf)

---

## **How do I run processing/production submission or any other data-flow command?**

**Solution:**
The first thing you can do is to check the scripts under **Processing & Production** in **SNO+ Data-flow Manual**, it gives specific desciption of when and how to use this command.
  
If you are still not sure about the format, a great suggestion is use `history | grep <comman_name>` on cedar. You can see the history of previous command users, try to find the pattern from the history and replace some past arguments with the one you need to use.
  
Before using the command, you have several things that need to check:
* Use `source env.sh` in the `data-flow` directory.
* Make sure you are in ths data-flow/gasp directory, otherwise you may need to change the path to the script.
  
As a last resort, the slack channel can be used as a fall back for sourcing information from other shifters and group members.

---

## **Where do I access the weekly usage plots for the DWG call?**

**Solution:**
The plots are automatically generated each week. You can find the latest ones on cedar, in ` ~/data-flow/gasp/monitoring/processing-plots` directory, using command `ls -ltrh`. The 3 plots needed have the following naming format “weekly-usage-X.png”.

---

## **Wrong CouchDB credentials & misformatted .json submission file**
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
**Production should ALWAYS be submitted with the data-processing-ug credentials. Use data-processing-ug as the user name!** If they aren’t submitted this way no jobs will be created. 
  
When there are incomplete or missing information errors produced with `production_submit.py`, this means that the spelling of the desired module in the .json file does not match the occurrence of that same module in the `production_information_<ratV>.py` in `data-flow/gasp/modules`.

---

## **Proxy does not have a production role error in Ganga**
DFLOW emails show that proxy does not have a production role. These errors are produced by the `gasp_client.py` code in data-flow and sent to snoplus_vosupport. 
  
**Solution:**
Run `voms-proxy-info --all` to check the proxy information. If the timeleft field shows 0, the proxy is not active and must be reset by Purvi or Deborah Morris (purvi@ualberta.ca, deborahmorris129@gmail.com)

If the timeleft field is non-zero and the production role error persists, please contact Jamie. 

---

## **Disk quota exceeded on Cedar**
If you are prompted with a “disk quota exceeded” message on Cedar, this likely means that either the `~/snoprod` or project space has exceeded the storage quota.

**Solution:**
Run `diskusage_report` to obtain a breakdown of allocated space for these directories. If we have indeed reached the limit, message the processing group in slack.

---

## **Need to resubmit canceled job, but there are several canceled passes**
If you need to resubmit a canceled job, the usual way is to use the `retry_failed.py` script from gasp. This will however resubmit all canceled passes for specific run / module combination at once with no option to select a specific pass. If you only need to resubmit the last pass, you can follow the solution here.
  
**Solution:**
Use 'retry_failed.py' with '--lastpass' flag and it will resubmit only the last pass.

---

## **Can’t connect to cedar1 but cedar5 is working (or vice-versa)**
Check https://status.computecanada.ca/ for any reports on cedar outages/issues
  
**Solution:**
Connect to a particular login node by specifying it at the start of the ssh host. For example , you can run 
```bash
ssh snoprod@cedar5.cedar.computecanada.ca 
```

---

**Note:** 
This works only at the sites enqueue_processing is running on a screen and it would make jobs with those ratv not run at all on any sites. This change is in conjunction with 
changing everything to get submitted through DIRAC as submission don't run though all different sites now.

---

## **The primary site for offsite transfers has a severe drop in efficiency. I want to switch the first site data is transferred to from SNOLAB.**
If the primary site (as of 2020, it’s SFU) has a severe network issue, it will prevent data moving from SNOLAB to the other 2 sites detector data is stored at. The data follows the path of UG to Primary Site (SFU) and then Primary Site to the 2 other storage elements used for raw data. 
  
**Solution:**
On snotflow@buffer1 we run a script called offsite_transfers, which performs the fts transfers to move data from SNOLAB to our storage sites. To adjust where data is transferred first (and therefore transferred from to the other 2 sites), one needs to modify the list called desination_ses inside the file `data-flow/dflow/offsite_transfers`. There are 3 elements in that list, all of which are `grid_transfers.StorageElement()` objects. Whichever StorageElement object is first in that list will be considered the primary SE. DO NOT remove any elements, simply adjust the order according to the desired outcome. 

---

## **Jobs are failing on Cedar due to “No route to host” error**
After Cedar has been down, the postgres db server may point to the wrong location i.e. sfu instead of snolab db (for both production and processing). 

The output error will be displayed as:
```bash
[ could not connect to server: No route to host
Is the server running on host "lcg-snodb.sfu.computecanada.ca" (206.12.127.61) and accepting
TCP/IP connections on port 5432?]
```

**Solution:**
In both the slurm production (`~/data-flow/gasp/config/production.cfg`) and processing (`~/data-flow/gasp/config/processing.cfg`) files, the path for postgres must be change from `#snopl_url=postgres://lcg-snodb.sfu.computecanada.ca:5432` (i.e. pointing to sfu) to the variable `snopl_url=postgres://pgsql.snopl.us:5400` (i.e. pointing to snolab db).

Some jobs are still likely to fail following this correction. However, running the retry_failed script (following the completion of the processing/production jobs) should apply the new settings and run correctly.

---

## **ImportError: No module named sites.[Your_site]**
Full Error Example:
```bash
File "/user/snoplusprod/ganga-7.1.9/bin/ganga", line 84, in <module>
  GangaCore.Runtime._prog.run(GPI.__dict__)
File "/user/snoplusprod/ganga-7.1.9/lib/python2.7/site-packages/ganga/GangaCore/Runtime/bootstrap.py", line 1098, in run
  execfile(script, local_ns)
File "bin/gasp_client.py", line 286, in <module>
  run_client(options)
File "bin/gasp_client.py", line 134, in run_client
  farm_doc = document.FarmDocument(db_module, check_location())
File "bin/gasp_client.py", line 46, in check_location
  site = gasp_util.load_location(config['location'])
File "/user/snoplusprod/data-flow/gasp/util/gasp_util.py", line 110, in load_location
  modobjct = __import__('sites.%s' % location)
ImportError: No module named sites.liverpool_dirac_processing
```

**Solution:**
Ensure the `__init__.py` file exists in whatever directory you are trying to load from, as Python 2 requires this file to exist in any directory that you try to load modules from (this init file is supposed to be empty). 

---

## **Completely missing subruns when trying to download ntuple/root files but run is in a completed state (on gasp and in job doc)**
Example error output when download attempted: `Error: ['[SE][Ls][SRM_INVALID_PATH] No such file or directory`

When checking the output files (of a run’s job document), the SRM path will be completely missing and there will be no listed guid. This indicates the subruns are completely missing. 

The issue’s origin is not completely known. However, this seemed to occur during a period where CEDAR was having filesystem issues. The subruns may be seen as “successfully” copied to the storage element but a failure has occurred. 

**Solution:**
Use gfal-ls SRM_PATH command (method outlined in troubleshooting doc https://www.snolab.ca/snoplus/private/DocDB/0059/005936/001/FS_Proc_Troubleshooting_8Aug.pdf). Note, as there is no SRM Path listed for this subrun, you can use the SRM path from a different subrun in the job document while substituting in the relevant subrun value (i.e. “_s00X_”). If the run is present then follow the steps in the troubleshooting doc. Otherwise, an error like this will be displayed: 

```bash
gfal-ls error: 2 (No such file or directory) - Error reported from srm_ifce : 2 [SE][Ls][SRM_INVALID_PATH] No such file or directory
```

If this error is displayed, delete the relevant runs using the `delete_jobs.py script`. Then, resubmit the runs using the `offline_processing.py` script (see here for information on running these scripts specifically nodes 26 and 22 respectively).

---

## **Prerequisites for processing/producing jobs with a new RAT version**
Jobs will not run unless there is a corresponding information file for the specific RAT version.

**Solution - Processing:**
Access the directory data-flow/gasp/modules/ and use a text editor to create a new file with the following format: 
`processing_information_X_Y_Z.py` -> where X, Y and Z correspond to the RAT version numbers
Usually, the information can then be copied from the previous RAT version’s processing information file. 
However, edits may need to be made to the copied information (in certain fields), particularly if a new module name is   being used or a previous macro is replaced by a new one.

**Solution - Production:**
To create a production information file, access the `gasp/bin/misc/` directory and use the make_production script. Specific information about the script can be found [here](https://snoplus.github.io/data-flow-documentation/data-flow/processing_and_production/subsec/misc_make_production.html)

---

## **Prerequisite for using a new processing module name (or new/updated information)**
Jobs will not run unless the information file contains the correct name for the specific RAT version.

**Solution:**
Access the processing information file (i.e. `data-flow/gasp/modules/processing_information_X_Y_Z.py`) , for the corresponding RAT version  X_Y_Z. The information contained in the file will resemble this:
```bash
"Processing": {
          "type": "macro",
          "level": "sub_run",
          "path": "mac/processing/neck_fill/second_pass_processing.mac", # first_pass_processing.mac runs nearline now
          "ganga_outputs": [],
          "outputs": ["ratds_blind"],
          "prerequisites": [],
          "raw_processor": True,
          "nhit_cut": -1
          },
```
Copy this and then change the appropriate fields with the new name or information. Note that certain fields are specific to processing or analysis modules. Therefore, copying  “Analysis” module information may be more appropriate in some cases. 

---

## **How do I extend VOMs memberships?**
Received an email from "VOMS Admin for VO snoplus.snolab.ca" <j.r.wilson@qmul.ac.uk> with the subject [VOMS Admin] Membership expiration notice for VO snoplus.snolab.ca. 
Emails received from this address could also be “User suspension notification”

**Solution:**
Follow the guide provided here (similar steps can be taken for user suspensions):
https://www.snolab.ca/snoplus/private/DocDB/0052/005281/001/2018.08.06_voms_KEG.pdf

---

## **How do I move a file from my local machine to cedar?**

**Solution:**
Go to your `.ssh` directory, try
```bash
scp -i <ssh key file> <file path> snoprod@cedar.computecanada.ca:<destination path>
```

---

## **Mixing ntuple and ratds path in the output files**

**Solution:**
If you do not have many jobs that need to be fixed, you can go to the job document on CouchDB and manually correct it by hand. In the ntuple output file, you need to add `.ntuple` in the specific subrun; in the ratds output file, you should remove `.ntuple` there. For the specific format, you can look at the path of other subrun.

If there are other jobs, consider to use the script `fix_ntuple_ratds_production.py` in `gasp/validation`

---

## **Multiple Processing Module runs have 2 job docs with the same input file and pass number**

**Solution:**
Delete either one of the job documents of **BOTH** modules in CouchDB, remember to delete the Analysis40 one first.

---

## **The screen is showing "unchanged from completed"**
This can happen if duplicate `gasp_client`'s are/were running on Cedar, a job got submitted by both, but then it finished on one (and the other can't figure out why the status is completed, but the job is still running).

**Solution:**
Reset the Ganga cache and registry:
1. Drain the queue on the site, and shutdown the `gasp_client`
2. Delete all files in the `gangadir` defined in the `.gangarc` for the site (ex. `gangadir = $HOME/gangadir_slurm_processing` for cedar processing)
3. Clean out the `jobdir` as well as `$TMP` for the site
4. Restart the `gasp_client`

**NOTES**
1. Caveat for Cedar - Processing and Production share a `$TMP` and `jobdir` so **DON'T** clean those out if you're not resetting both Ganga's
2. If no jobs are running, draining the queue may not be necessary so skip that step and shut down the client, and continue

---

## **CGSI-gSOAP running on buffer1.sp.snolab.ca reports could not open connection to lcg-snopse1.sfu.computecanada.ca:80**

A full example of the error:
```
Error:  ['gfal-sum error: 70 (Communication error on send) - srm-ifce err: Communication error on send, err: [SE][Ls][] httpg://lcg-snopse1.sfu.computecanada.ca/srm/managerv2: CGSI-gSOAP running on buffer1.sp.snolab.ca reports could not open connection to lcg-snopse1.sfu.computecanada.ca:80', '', '']
```
This is what we call the "**port 80 issue**" and can be identified when the **SE** URL contains **:80** at the end (as this one does). That is the incorrect port, so things will certainly fail.

**Solution:**
Change the BDII to the CERN one. This can be done by editing the **.bashrc** (located in `~/.bashrc`) on **buffer1**. If you open the file, you will see some lines like this:
```bash
#export LCG_GFAL_INFOSYS=lcg-bdii.cern.ch:2170
export LCG_GFAL_INFOSYS=lcgbdii.gridpp.rl.ac.uk:2170
export MYPROXY_SERVER=myproxy.gridpp.rl.ac.uk
#export MYPROXY_SERVER=myproxy.cern.ch
```
Change them so that the **gridpp** ones are **commented out** and then **uncomment** the **CERN** ones like so:
```bash
export LCG_GFAL_INFOSYS=lcg-bdii.cern.ch:2170
#export LCG_GFAL_INFOSYS=lcgbdii.gridpp.rl.ac.uk:2170
#export MYPROXY_SERVER=myproxy.gridpp.rl.ac.uk
export MYPROXY_SERVER=myproxy.cern.ch
```
The reason this works is because the BDII is a database index which contains the correct storage locations; the RAL one seems to incorrectly have the port set to 80, hence why changing to the CERN one fixes it.

---

## **How do I cancel transfers on buffer1?**

**Solution:**
Sometimes transfers from buffer1 can have issues, and changes may need to be made to offsite_transfers to reduce the number of connections. After doing this, you can either wait for the current transfers to all fail after being checked, or to expedite the process, you can manually cancel them by doing the following:

1. Navigate to [this CouchDB view](https://couch.snopl.us/_utils/#/database/data-processing/_design/dflow/_view/raw_data_transfers) to see current ongoing transfers
2. Notice that many of the same document ID may be visible - this is due to it listing each subrun individually, which all may be within the same document consisting of the whole run
3. Navigate into a document, and look through the subruns until you find one with a `transfer` section. In this section will be an `id` field - copy that ID
4. On buffer1, run the following command:
```bash
fts-transfer-cancel --proxy /home/snotflow/grid/proxy/production_proxy -s https://lcgfts3.gridpp.rl.ac.uk:8446 [TRANSFER_ID]
```
where [TRANSFER_ID] is the ID you previously copied. It should say "CANCELLED" indicating success
5. Repeat these steps for each unique ID in the document(s)

---

## **Nothing in CouchDB is loading in any of the views**

**Solution:**

This usually happens if you leave couch open in a tab for too long. Close the tab and open a new one, and couch should start working again.

Otherwise, if you created a new view on Couch it takes about 15-25 minutes for it to reload everything.

---

## **Where do the gasp monitoring pages get their information on job status?**

**Solution:**

[This is the view](https://couch.snopl.us/_utils/#/database/data-production/_design/dproc/_view/job_passes_by_status_module_reduce) for **production** and [this is the view](https://couch.snopl.us/_utils/#/database/data-processing/_design/dproc/_view/job_passes_by_status_module_reduce) for **processing**. To see the totals for each status, go into **Options** in the **top right** of the page, click **Reduce**, then change the **Group Level** to **1**. If you want to see the statuses broken up by module, change the **Group Level** to **2**.

---

## **There are jobs waiting, but nothing is being picked up on Dirac or Cedar!**

**Solution:**

This is a pretty general issue and could be caused by many things, but some common fixes to try:

1. Ensure that the correct type of job is set in the **launch_processing.sh** script (there are variables at the top for processing and production, ensure the appropriate ones are set for the type you want to run). This is usually only an issue on Dirac as Cedar is set to run both types always.

2. Reset the screens on Cedar and Liverpool - just kill them, and rerun **launch_processing.sh**. Attach to the screens and watch what happens; you can tell this fixed it as **enqueue_production** / **enqueue_processing** (whichever you are trying to run) should show it as submitting and running jobs.

3. Make sure transfers on buffer1 are working - this can quickly be verified by looking at Grafana and ensuring all 3 sites show high transfer efficiency; however, you should also check the screens running on buffer1 as well just to ensure there are no errors popping up. If the raw data can't get out of SNOLAB, it can't be used to fulfill the prerequisites for processing (and likewise, run-by-run production also won't have the runs appropriate to simulate off of).

If all of this fails, consult Deborah Morris (deborahmorris129@gmail.com) or Purvi (purvi@ualberta.ca).

---

## **The power went out at SNOLAB and weird things are happening with CouchDB - what should I do?**

**Solution:**

Couch replicates to another instance above ground, and if the power goes down underground, this replication will halt. If the screen sessions weren't stopped prior to the outage, weird things will begin to happen. The replications pending will probably go up significantly during the outage, which one can [view here](https://snopl.us/network/dbstatus). If the power returns and the replications pending don't go down (they should be around 0 if things are working) please email the networking group at snoplusnetwork@googlegroups.com to let them know that the couch processes may need to be restarted on the systems.

---

## **How should data-flow be updated on our sites?**

**Solution:**

The git workflow for data-flow is the following:

1. **Never** change any of the master branches except with a Github merge
2. Only run `git checkout master` and `git pull origin master` on the sites, to update them

These two steps will keep our master clean and avoid git issues from making local changes/commits (which will result in a divergance).

---

## **Transfer efficiency is very low in Grafana, and buffer1 reports "SSL Connection" / "403 Forbidden" Errors**

**Solution:**

This means that there is an issue with the **File Transfer Service** (FTS), which is provided to us by RAL. There isn't anything we can do from our side, and this is a serious issue, so submit a ticket through GGUS specifying GridPP RAL and ensure you set the priority of the ticket to **top priority**.

---

## **Instruction for experiencing outage in Snolab**

**Solution:**

Kill all the screen sessions on cedar/dirac
* `screen -r` to the current screen session
* `ctrl-a-k` then `y` to kill the current tab 
* OR screen -X -S <session # you want to kill> kill

---

## **How to run different types of jobs on different sites**

Sometimes we want to run "split site mode" and submit different jobs (incoming processing/analysis, reprocessing and production) on different sites.

**Solution:**

`diracSubmitScript.py` was updated and we have the following info available to us just before any job gets submitted:
- Rat version
- Module
- Run number

If we have any reprocessing/production jobs that we would like to run on specific sites, we would be able to filter them using the above attributes. 

---

## **Stop a jobs with a specific RAT version to run of any site**

**Solution:**

Comment out the RAT version under `rat_locations` inside `<site>_processing..py` file in `data-flow/gasp/sites` directory on the site where `enqueue_processing` screen session is running.

---

## **Where can I add processing or production requests?**

**Solution:**

This is the [site](https://snopl.us/production/) used to add and send processing and production requests

---

## **Bad run number causing data transfer efficiency issues**

**Solution:**

If you are having efficiency issues with data transfer **and** there are jobs with a very large run number failing to get transferred from buffer1, these are bad runs that need to be deleted. 

- Delete the files on buffer1 (you might find it under /raid/data/l1/bad_run_number/, grep the file to find which directories it is under. It can exist under multiple dirs). 
- Delete their corresponding data docs on CouchDB as well.

Changes to `offsite_transfer` have been made to warn and skip these very large bad runs. 
