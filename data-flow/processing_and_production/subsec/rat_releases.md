---
layout: default
title: RAT releases
parent: Processing & Production
grand_parent: SNO+ data-flow manual
nav_order: 3
---

# RAT releases

When a new RAT release is issued the following must be done before processing or production can run with the new software:
* [Install](./install.md) the tagged software on CVMFS.
* Production macros have been [benchmarked](./benckmark.md) (may not be necessary if minor change in RAT and macros are not new).
* [Generate](./generating_info_files.md) new processing and production information files.
* [Update](./updating_clients.md) the sites file for and pull the latest data-flow (with added production/processing information files) for each client location.