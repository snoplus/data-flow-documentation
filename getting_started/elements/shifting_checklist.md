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
  
* Production or reprocessing request
  You may receive production or reprocessing requests like the following:
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
  Production requests will be followed with attachments, usually a .json file and a .txt file. You should `scp` that you temporary directory on Cedar or Directory and run the following command for submission. Remember to source the environment script in `data-flow` first.
  `python bin/production_submit.py ~/$TEMP_DIR/<file>.json -t <Rat DB Tag> -o ~/$TEMP_DIR/<file_name>.exactly -L ~/$TEMP_DIR/<file_name>.txt`
  
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
  Reprocessing requests will also be followed with attachments, usually a .txt runlist. Remember to check if the module exists in process_information_X_Y_Z.py in `data-flow/gasp/modules` first. Possible command for submission is:
  `python bin/offline_processing.py config/<site>.cfg <ratV> L1 -t <Rat DB Tag> -N <Second Pass Module> -N <Third Pass Module>  -L ~/$TEMP_DIR/<file_name>.txt -o ~/$TEMP_DIR/<file_name>.exactly`

* Failed jobs resubmission.
  We have a automatic script `~/cron/retry_jobs.sh` that will resubmit some failed jobs that satisfy some specific requirements twice a day, but there are still some other jobs that have to be submitted manually.
  * Identify issues.
    We usually have email notifications turned on for failure jobs, the execution log will be attached along with the email. If the email includes `Note, no attachments for error/output logs!`, this means the job has no execution log, this usually happen when a job fails before actually running, which can be solved by resubmission. If you want further information, you can get `dirac_id` in its data document, and search that on [Dirac monitoring page](https://dirac.gridpp.ac.uk:8443/DIRAC/).
    
* Screen sessions
  We use screen sessions to monitor our jobs, screen sessions only exist on cedar1 and Dirac. You can use `screen -r` to see it. Sometimes jobs are stalled because screen sessions get stuck, you can restart the screen session to fix it. You can find the step for killing the screen sessions in SNO+ issues and solutions guide.
  
* Graham monitoring
   We have email alert when the transfer efficiency drops on Graham, you sould check Graham when you receive the alert.
  
  
### Weekly Checklist
* DWG meeting.
  You need to attend weekly DWG call at 2:00 p.m. and give some information about shifting of this week in the data-flow section. Sign up the mailing list, snoplus@snolab.ca, for further information.
  
* Fill the [shift report](https://www.snolab.ca/snoplus/TWiki/bin/view/DAQ/ListOfAllDataFlowReports) every week.
    






