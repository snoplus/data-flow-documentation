---
layout: default
title: Overview
parent: Data-flow
nav_order: 1
---

# Overview

Data-flow is responsible for transporting and archiving raw data from SNO+ to distributed Grid storage nodes. SNO+ data from the builder (L1 data) and L1 data that has been processed by the L2 software trigger (L2 data) are both stored on buffer1. For water data, we preserve all L1 and L2 files, however, there may come a time when only the L2 files are kept and only a fraction of the L1 files are retained. Data-flow picks up data at this stage, registers the data in the “data-processing” database (including size and checksum information) and submits transfer requests to a file transfer scheduler (Grid-FTS). Once a replica of the data is available at the first Grid storage node further transfer requests are submitted to transfer the data from Grid node 1 to two further sites. When a file has been replicated (with checksum confirmation) to three Grid sites the copy on buffer1 is deleted. The current replica sites are SFU (Canada), FNAL (US), and RAL (UK). The storage at RAL is the permanent archive and typically sent to tape.

All L2 data is archived permanently (in zdab format); a fraction of L1 data is archived. Data from the builder and L2 trigger are written to `/raid/data/l1` and `/raid/data/l2` respectively. Older data is accessible on the grid via their guids, however newer data can be listed with `lfn:/grid/snoplus.snolab.ca/snotflow/raw/l1` or `lfn:/grid/snoplus.snolab.ca/snotflow/raw/l2`. Log files from the data runs are also preserved (in log format) and are listed in `lfn:/grid/snoplus.snolab.ca/snotflow/raw/log`.
