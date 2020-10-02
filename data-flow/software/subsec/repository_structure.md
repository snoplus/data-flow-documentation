---
layout: default
title: Repository structure
parent: Software
grand_parent: SNO+ Data-Flow Manual
nav_order: 1
---

# Repository structure

The production software is divided up into the following structure:

* data-flow/lib/dflow: Helpfully named, contains modules useful for both dflow and processing (“gasp”), including the document.py module that defines document structures used by the data-flow / processing and production databases. 
* data-flow/lib: More modules used by both dflow and production, but potentially also useful externally (e.g. lcgutils is a set of simple wrapper functions to call lcg-utils command line executables). 
* data-flow/dflow: the dflow code. 
* data-flow/gasp: the processing and production code: 
  * data-flow/gasp/GangaSNOplus: the SNO+ ganga plugin. 
  * data-flow/gasp/core: core stuff! 
  * data-flow/gasp/modules: specific setup for different production and processing modules 
* data-flow/database: database structure (design documents, attachments for monitoring), layout follows the requirements of [stool](https://github.com/mjmottram/stool) to push the design document structure to a CouchDB database. 
* data-flow/grid: contains examples on how to setup proxies etc. 
* data-flow/doc: what you're reading now. 
