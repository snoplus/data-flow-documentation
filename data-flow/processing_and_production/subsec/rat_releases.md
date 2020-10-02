---
layout: default
title: RAT Releases
parent: Processing & Production
grand_parent: SNO+ Data-Flow Manual
nav_order: 3
---

# RAT releases

When a new RAT release is issued the following must be done before processing or production can run with the new software:
* [Install](./RAT_releases/install.md) the tagged software on CVMFS.
* Production macros have been [benchmarked](./RAT_releases/benckmark.md) (may not be necessary if minor change in RAT and macros are not new).
* [Generate](./RAT_releases/generating_info_files.md) new processing and production information files.
* [Update](./RAT_releases/updating_clients.md) the sites file for and pull the latest data-flow (with added production/processing information files) for each client location.
