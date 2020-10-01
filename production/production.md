---
layout: default
title: Production Overview
nav_order: 4
has_children: false
has_toc: false
---

# **Production Overview**

This is a high-level overview of the steps that it takes to produce Monte Carlo simulation, starting from retrieval a production request, all the way to the final storage of it. An infographic is supplied at the bottom of the page (courtesy of Juan Pablo Yanez) that will hopefully help visualize this flow of data.

---

## **Monte Carlo Production**
1. Collaboration decides on important Monte Carlo dataset to generate (in the form of a **Production Request**)
2. The Production Request, available [here](https://snopl.us/production/production-request), will inform:
  * What **macros** (also known as **modules**) need to be run, which in turn decides the **event mode**
  * Whether it is **run-by-run** (pass in specific run information to recreate exact detector conditions) or **generic** (all the other modes; use default detector values)
  * How many events are simulated
3. Once the request is filled out and submitted, the emails listed in the contact portion and the **snoplus_vosupport** mailing list will receive the request which will have attached the created **jobs.json** file, containing all of the request details in the necessary format and ready to be submitted by `production_submit.py`
4. Submit this jobs file using `production_submit.py` with the appropriate arguments (consult the data-flow manual for that script for specific information)
5. When submitting production, an "**.exactly**" file is created which contains the module, run and pass number for each job in that production campaign
  * During the Production Request, there is a **Production Label** field; this is used to tag each job (and corresponding job doc) with that label, so that it is easy to find the data later on as part of that campaign. One can also use labels with `grabber.py` to download data by label, and also produce an .exactly file by using a label
6. Jobs will get submitted to a specific site, and the **enqueue_production** screen takes over to monitor
  * **enqueue_production** works very similarly to **enqueue_processing**
  * They **both** run `gasp_client.py`, where one looks at the **data-production** database on CouchDB, while the other looks at the **data-processing** database; the two databases are almost identical in that they utilize the same layout of views, but the content is naturally different
7. When a job's status is "completed", the output data will be saved on the specified Grid **storage element** (or **SE** for short; ex. lcg-snopse1 at SFU)

---


## **Visualization of the Production chain**


![production chain image](../assets/images/production-flow.png)


