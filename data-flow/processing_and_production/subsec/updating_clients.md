---
layout: default
title: Updating clients
nav_exclude: true
---

## Updating clients

Once module information has been added to the data-flow git repository, all sites submitting processing and production jobs will need updating. First ensure that the client is on the master branch, if it is not, find out why by asking the group before continuing. Then perform a `git pull origin master` to pull the remote origin code into the local master branch. On most clients you can use your own github username and password. On sfu (snoui01.sfu.computecanada.ca), there is an ssh key set up with a special password which can be found in /GitHub.txt.

Then update the sites/[site].py file so that the “rat_locations” field includes an entry for the new software (if using software via CVMFS then this should be set to “CVMFS”).

