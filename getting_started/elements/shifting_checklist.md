---
layout: default
title: Shifting Checklist
parent: Getting Started
nav_order: 5
---

## **Shifting Checklist**

### Daily Checklist
* Approve or Extend VOMS membership

  Email notifications will be sent out when there are requests need to be approved or extended.
  Click [here](https://voms.gridpp.ac.uk:8443/voms/snoplus.snolab.ca/user/search.action) for membership extension
  Click [here](https://voms.gridpp.ac.uk:8443/voms/snoplus.snolab.ca/home/login.action) for membership approvement.
  Links will also be included in the email, you must be the admin first before doing above actions
  
------------------------------------------------------------------------------------------------------------------------------------------------------------
  

* Screen sessions

  there are a total of six screen sessions you need to worry about. You can use `screen -r <screen name/Id>` to see them. you can use `-x` to see a screen someone else is currently looking at.

  On Buffer1:
    **dflow_register** registers new incoming raw files
    **offsite_transfers** handels the transfers from buffer 1 to the grid storage.

  On cedar1:
    **benchmarking** submits benchmarking jobs to cedar
    **offline_processing** creates and inserts processing jobs to the database for incoming data.

  On liverpool:
    **enqueue_processing** submits and monitors processing Dirac jobs through ganga.
    **enqueue_production** submits and monitors production Dirac jobs through ganga.

  If the screens on B1 need to be relaunched, first kill them and then run `~/data-flow/cron/launch_processing_screen/launch_dataflow.sh`. 
  if the other screens need to be relaunched go to the [cedar](https://github.com/snoplus/data-flow/actions/workflows/start-screens-cedar.yml) or [liverpool](https://github.com/snoplus/data-flow/actions/workflows/start-screens-liverpool.yml), this will kill and relaunch the screens on that site. If the action says `This workflow was disabled manually.` ssh to the site, kill the screen, git pull and run `~/data-flow/cron/launch_processing_screen/launch_processing.sh`, the variables in `launch_processing_settings.sh` denote which screens will be launched, so modify them to true/false accordingly.
  
------------------------------------------------------------------------------------------------------------------------------------------------------------

* Production requests
  
  You may receive production request like the following:
  ```
  Production Request for:
  Rat Version: 6.18.9
  Rat DB Tag: partialfill-physics-v2
  Priority: medium
  Total CPU-Year Request: 1.6
  Run-by-run: Attached
  Further Information:
  With the ratdb tag.
  ```
  Production requests will be followed with attachments, usually a .json file and a .txt file. You should copy those to Cedar into a temporary directory:
  ```bash
  scp <file1> <file2> username@cedar.computecanada.ca:~/<my_temp_dir>
  ```
  if the file is short you can just copy it through the termanal. 
  Once the files are on Cedar, source the data-flow environment:
  ```bash
  cd ~/data-flow
  source envpy3.sh
  cd gasp
  ```
  Now you are ready to submit the request:
  ```python
  python bin/production_submit.py -t <Rat DB Tag> -o ~/<my_temp_dir>/<file_name>.exactly -L ~/<my_temp_dir>/<file_name>.txt config jobfile
  ```
  where `<file_name>.exactly` is the name of the output file, containing a list of all of the things submitted. It is important to keep track of these files, as they are referenced by members of the group so consider making a separate directory for storing these.
  Just specify the config and jobfile name. DO NOT type out config or jobfile then the name of the file. 
  
------------------------------------------------------------------------------------------------------------------------------------------------------------

* Reprocessing requests

  A reprocessing request looks similar to a production request:
  
  ```
  Reprocessing Request for:
  Rat Version: 6.18.9
  Priority: high
  Rat DB Tag: partialfill-physics-v2
  Run-by-run: Attached
  Further Information:
  Submit once the Analysis40R is done (including the resubmission of the partfailed).
  Second pass processing: https://github.com/snoplus/rat/blob/master/mac/processing/neck_fill/second_pass_processing.mac
  Third pass processing:
  - name = Analysis40R_CrOff
  - nhit cut = 40
  - macro = https://github.com/snoplus/rat/blob/master/mac/processing/neck_fill/third_pass_analysis_processing_classifier.mac
  ```
  most of them can now be submited through the action, go [here](https://github.com/snoplus/data-flow/actions/workflows/submit-reprocessing-list.yml), copy the contence of the `.txt` file into `the list of run numbers`, select the ratv and other paramters and click `run workflow`
  if any of the options are missing edit the file [here](https://github.com/snoplus/data-flow/blob/master/.github/workflows/submit-reprocessing-list.yml) to add them.

  here are the instructuons to do it manualy:

  Reprocessing requests will also be followed by attachments, usually a `.txt` runlist file. Copy this to Cedar into a temporary directory:
  ```bash
  scp <file1> username@cedar.computecanada.ca:~/<my_temp_dir>
  ```
  Double-check that the processing module file exists in `~/data-flow/gasp/modules/processing_information_X_Y_Z.py`, where `X_Y_Z` is the RAT version that will be used. For example, `processing_information_6_18_9.py` for RAT 6.18.9
  
  If it is not there, then it needs to be created. Since little changes between the modules, the process to create a new one is just:
  - Make a copy of the latest `processing_information_X_Y_Z.py` file
  - Rename it to include the latest RAT version
  - Ask Valentina Lozza about any new changes to include, and add them in
  
  Once the file is on Cedar and the module is in the right place, source the data-flow environment:
  ```bash
  cd ~/data-flow
  source envpy3.sh
  cd gasp
  ```
  Now you are ready to submit the request:
  ```python
  python bin/offline_processing.py config/<site>.cfg <ratV> L1 -t <Rat DB Tag> -N <Second Pass Module> -N <Third Pass Module>  -L ~/<my_temp_dir>/<file_name>.txt -o ~/<my_temp_dir>/<file_name>.exactly`
  ```
  where `<file_name>.exactly` is the name of the output file, containing a list of all of the things submitted. It is important to keep track of these files, as they are referenced by members of the group so consider making a separate directory for storing these.
  
  A **module** is slightly different than a **macro** - the easiest way to find out what the corresponding module is to the given macro is to look it up in the **processing_information_X_Y_Z.py** for that RAT version; the file is basically just a giant mapping of modules to macros with given arguments.
  
  **Note:**
  1. Use `-f` only when you need to set Analysis job's Processing prerequisite to the largest (most recent) pass number of Processing for each run instead of requiring it be part of      the submission.
  2. Sometimes you need to submit both processing and analysis jobs with these processing ones being prerequisite for the analysis. Use `-N <Processing module> -N <Analysis module>`      to submit them all together. It is best to do this in one step, specifying all modules, than submitting processing modules and then submitting analysis modules which won't always work. 
  You don't need `-f` flag in this case.

------------------------------------------------------------------------------------------------------------------------------------------------------------

* Failed jobs resubmission

  We have a automatic script `~/cron/retry_jobs.sh` that will resubmit certain failed jobs that satisfy some specific requirements twice a day and retry_failed.py has been integrated   into gasp_client to resubmit part failed and failed jobs **only after run 270000**; this works for both Processing as well as Productions jobs. But there are still some other jobs that have to be submitted manually.
  * Part Failed jobs
    Sometimes we need to resubmit part failed jobs. Use retry_failed.py to resubmit them instead of using offline_processing.py which would give these jobs a new unwanted pass.
  * Identify issues.
    We usually have email notifications turned on for failure jobs, the execution log will be attached along with the email. If the email includes `Note, no attachments for error/output logs!`, this means the job has no execution log, this usually happen when a job fails before actually running, which can be solved by resubmission. If you want further information, you can get `dirac_id` in its data document, and search that on [Dirac monitoring page](https://dirac.gridpp.ac.uk:8443/DIRAC/).

------------------------------------------------------------------------------------------------------------------------------------------------------------

* draining the queue

  If something goes wrong with the jobs or something you may need to remove all the jobs from the clusters:
  **processing**:
  ```bash
  cd ~/data-flow/tree/master/cron
  screen -S drain ./processing/enqueue_processing_drain.sh
  ```  
  **production**:

  ```bash
  cd ~/data-flow/tree/master/cron
  screen -S drain ./production/enqueue_production_drain.sh
  ```

------------------------------------------------------------------------------------------------------------------------------------------------------------

### Weekly Checklist
* DWG meeting

  You need to attend weekly DWG call on Tuesday at 2:00 p.m. EST and give updates on anything notable that happened during the last week. Sign up to the mailing list, snoplus@snolab.ca, for further information.  
