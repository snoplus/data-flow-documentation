---
layout: default
title: Adding new grid resources
parent: Grid
grand_parent: SNO+ data-flow manual
nav_order: 4
---

# Adding new grid resources

To add a CE and/or SE that the snoplus.snolab.ca VO can access the following should be tested: 
* The site publishes information on both CE and SE: 
  * lcg-infosites ---vo snoplus.snolab.ca ce (se) will list all CEs (SEs).
  * lcg-info ---list-ce (---list-se) ---attrs <attr> will list specific attributes.
  * CEs need to list valid results for MaxCPUTime, Memory, VMemory.
  * SEs need to list valid results for VOInfoPath.
* Jobs can be submitted from Dirac; Contact Daniela Bauer (daniela.bauer@imperial.ac.uk) to add the resource to the GridPP Dirac instance and to test pilot jobs 
* Appropriate environment variables are present when running jobs on the backend: 
  * VO_SNOPLUS_SNOLAB_CA_SW_DIR: should point to the snoplus.egi.eu CVMFS repository (typically /cvmfs/snoplus.egi.eu)
  * VO_SNOPLUS_SNOLAB_CA_DEFAULT_SE: should refer to the appropriate local storage element.
