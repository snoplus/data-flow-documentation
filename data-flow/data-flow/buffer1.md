---
layout: default
title: Buffer1
parent: Data-flow
nav_order: 1
---

# Buffer1

The machines buffer1 and buffer2 (which is a backup) run the builder, L2 trigger, and data-flow scripts. Buffer1 sits on both the SNO+ private network ([buffer1.sp.snolab.ca](buffer1.sp.snolab.ca)) and the SNO+ data network ([buffer1.spdata.snolab.ca](buffer1.spdata.snolab.ca)). In addition, there is a public interface ([snoplusdata.snolab.ca](snoplusdata.snolab.ca)) which is used so that the Grid-FTS can access and transfer data.

Data-flow services run on the “snotflow” user account. Offsite access requires a VPN and should be made via the buffer1.sp.snolab.ca interface; see the [SNO+ computing manual](https://snopl.us/detector/documents/snoplus_computing_manual.pdf) for more information.

A Grid-FTP (Grid File Transfer Protocol) server runs on buffer1; the setup of this server is documented in [DocDB 3692](https://www.snolab.ca/snoplus/private/DocDB/cgi/ShowDocument?docid=3692). SNOLAB firewall and the buffer1 network settings are configured to restrict access to the public snoplusdata.snolab.ca interface to the ports required by the Grid-FTS. In addition, the buffer1 Grid-FTP configuration is setup to ensure that any transfers from the Grid-FTS only have read access to the /raid/data directory and no access to any other paths.
