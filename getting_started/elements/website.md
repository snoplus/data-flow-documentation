---
layout: default
title: Websites
parent: Getting Started
nav_order: 3
---

## **Websites**

This page will introduce some useful websites you may need for processing, production and monitoring.

---

### **Monitoring Sites**
There are monitoiring sites you can use to monitor production and processing job status. Some of the websites require a [GRID certificate](./tools.md#grid-certificate) and a [SNOLAB account](./account.md#snolab).

#### **Dirac**
This is an interactive monitor to view jobs running on the Dirac platform. You can even view the inputs and outputs of individual jobs to assist in debugging.

[https://dirac.gridpp.ac.uk:8443/DIRAC/](https://dirac.gridpp.ac.uk:8443/DIRAC/)

#### **Monitoring for processing**
This is an internal tool to track processing jobs, similar to the above, but for jobs running on Cedar.

[https://couch.snopl.us/data-processing/_design/dproc/index.html](https://couch.snopl.us/data-processing/_design/dproc/index.html)

#### **Monitoring for production**
Same as above, but for production.

[https://couch.snopl.us/data-production/_design/dproc/index.html](https://couch.snopl.us/data-production/_design/dproc/index.html)

#### **Granfana**
Grafana shows us the transfer status of data from SNOLAB being triplicated to our three primary sites. If transfer efficiency drops for a significant period of time, there is an issue with one of the sites. 
* To login, click the following link, and choose "CERN Single Sign On", and on the next page, click on "Guest Access". Then, use the processing account - please inquire to the processing channel on Slack for more information.

[https://monit-grafana.cern.ch/login](https://monit-grafana.cern.ch/login)

---

### **Resources**

#### **GGUS**
GGUS is where requests are sent for issues with our **RAL** (UK) storage site, **Liverpool** node, **CVMFS**, and **Dirac** system. Please see the GGUS section of the [Contact List](../../communication/list.md) for more information.

[https://ggus.eu/](https://ggus.eu/)

#### **Twiki**
TWiki is the wiki for SNO+. It contains some useful information like:
* [SNO+ Credentials](https://www.snolab.ca/snoplus/TWiki/bin/view/Main/SystemPasswords)
* [Shift schedules and reports](https://www.snolab.ca/snoplus/TWiki/bin/view/DAQ/ProcessingShifts)

#### **DocDB**
DocDB is a database of collaboration presentations, documents, slideshows, and other useful information. Here are some resouces you might want to read:
* [Home page](https://www.snolab.ca/snoplus/private/DocDB/cgi/DocumentDatabase)
* [Shifter Responsibility List](https://docs.google.com/document/d/1Xko1J3ETjqFuRP1p_8660YQuBnY6dh_5X4irZpalVWU/edit)
* [VOMS Membership](https://www.snolab.ca/snoplus/private/DocDB/0052/005281/001/2018.08.06_voms_KEG.pdf)
* [Common Issue/Shifter Troubleshooting](https://www.snolab.ca/snoplus/private/DocDB/0059/005936/001/FS_Proc_Troubleshooting_8Aug.pdf)
* [CouchDB Tutorial](https://www.snolab.ca/snoplus/private/DocDB/0049/004937/001/2018-02-22_Land_dataflow_database.pdf)

#### **Issues & Solutions from Data-Flow Shift Reports**
* [The 3 most recent Data-Flow shift reports](https://www.snolab.ca/snoplus/TWiki/bin/view/DAQ/DataFlowReport)
* [All Data-Flow shift reports (since September 2015)](https://www.snolab.ca/snoplus/TWiki/bin/view/DAQ/ListOfAllDataFlowReports)
* [A list of common shifting issues and solutions](../../processing_solution/instruction.md)

