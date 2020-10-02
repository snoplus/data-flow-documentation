---
layout: default
title: Site tests and issue ticketing
parent: Grid
grand_parent: SNO+ Data-Flow Manual
nav_order: 7
---

# Site tests and issue ticketing

Grid jobs may fail at a number of points during their life and due to a variety of reasons. First ensure that the issue is with a particular CE, then open a grid ticket The main resource for notifying sites of problems (be they related to workload management services, grid storage or worker nodes) is [GGUS](https://ggus.eu/index.php). GGUS tickets may be filed by any Grid user, however, the system should only be used if you have tested all simple solutions locally and are reasonably sure that the problem is being caused by (and can be solved only by) sysadmins at another site. Also ensure that there is no already existing ticket for the problem you are experiencing
 
If you are confident that you should be filing a ticket, please ensure that you: 
* Select an appropriate site (e.g. UKI-SouthGrid-Oxford if your problem is with jobs failing when they are submitted to an Oxford worker node). 
* Check for any downtime notifications at the site you are filing the ticket against (a window with this information will pop up when you select the appropriate site). 
* Check on the ticket's progress regularly (or turn on notificiation upon every change when submitting) and respond promptly when necessary to ensure that tickets are solved as quickly as possible (and to maintain a good reputation for the SNO+ VO with grid sites). 

The following instructions are possibly deprecated. Proceed with caution...

A framework has been developed consisting of simple tests checking common causes for job failures. Currently, the [GridTest](https://github.com/mjmottram/grid_tests) code is divided into two main parts investigating the job submission to the computing elements (CE) and confirming the software status on the grid. Ideally, these tests should be run at least once a week. 

The shell script submit.sh in the [site test](https://github.com/mjmottram/grid_tests/tree/master/basic_site_wms_test) folder submits a simple hello-world job to all sites to test the job submission. If only a specific site should be checked, execute submitSite.sh instead. The status of the jobs can then be monitored using glite-wms-job-status -i allCEsJobIDFile (when testing all sites) or singleSiteIDFile (when checking only a specific site). Further information and example tests (e.g. for Proxys) can be found on the [GridPP Wiki](https://www.gridpp.ac.uk/wiki/Some_simple_test_jobs). Additionally, check the [Nagios](https://vo-nagios.physics.ox.ac.uk/nagios/cgi-bin/status.cgi?servicegroup=VO_snoplus.snolab.ca&style=detail) website which shows any known problems for all CEs. 

To run the software checks use the scripts in the [cvmfs test](https://github.com/mjmottram/grid_tests/tree/master/cvmfs_test) folder. The file cvmfs_test.py needs to be run in ganga, whilst both check_system.py and myscript.sh need to be submitted to the grid backends (see [Operation](add later) to find information on submitting jobs to the grid). 
 
If a problem with one or multiple sites could be established, please execute set_ce_status.py in ganga. This script loops through every CE listed in the [CouchDB](http://snoplus.cpp.ualberta.ca:5984/_utils/database.html?test_grid) database and asks the user if they want to exclude them. **NB: This database no longer exists!** The -s flag can be used to force the script to only loop through a subset of sites (e.g. -s qmul.ac.uk will allow the user to only set the status of the QMUL sites). Excluding a CE sets its permission status on CouchDB to False. Once the problem with the site has been fixed, please re-set the permission status of the CE in question to True using the same script. 
