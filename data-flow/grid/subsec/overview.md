---
layout: default
title: Overview
parent: Grid
grand_parent: SNO+ data-flow manual
nav_order: 1
---

# Overview

The LHC Computing Grid (LCG) consists of distributed resoures that include Computing Elements (CEs), Storage Elements (SEs), and Workload Management Services (WMSs) among (many) other things.

There are Tier 0, 1, 2, and 3 grid sites. Tier 0 is Cern, Tier 1 are sites used for longterm data storage, Tier 2 are smaller sites that can hold small subsets of data but may perform large amounts of processing. Tier 3s are university clusters or user Grid UIs. This syntax is all based on experiments at Cern; for SNO+ purposes our Tier 0 is SNOLAB (even though it cannot hold an archived data set), Tier 1s are RAL (UK), SFU (Canada), and FNAL (US) and other computing sites are Tier 2s.

All Tier 1 sites run dCache systems, meaning that data stored there is backed up on tape and eventually will only be removed from the disk cache (with the removal based on last access time typically). For users or processing to access data on tape it must be recalled from disk, this process is automatic when e.g. running gfal-copy to download the data, however if large amounts of data need to be accessed then a bulk request to cache to disk may be more efficient. If attempting to access via XRootD or grid-FTS then data must be cached first (e.g. using `gfal-legacy-bringonline`). 
