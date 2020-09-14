---
layout: default
title: Account access
nav_exclude: true
---

## Account access

There are currently three locations running production clients:
* **snoplusprod@snoplusprod.ph.liv.ac.uk (Liverpool)**
* **snoprod@cedar.computecanada.ca (Canada Supercomputing Cluster)**
As far as possible they should have similar directory structures, such that processes needed to run a production client are found in the same place regardless of machine. Access for the two accounts is as follows:

**snoplusprod.ph.liv.ac.uk**

SSH keyed access, contact production list and fay@hep.ph.liv.ac.uk and jbland@hep.ph.liv.ac.uk to install new ssh key on the machine
```bash
ssh snoplusprod@snoplusprod.ph.liv.ac.uk # Or a simple ssh alias
```
The snoplusprod account at Liverpool runs subission via Dirac.

**cedar.computecanada.ca**

If you do not have a user account with Compute Canada, you will need to apply for one (guide [here](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/)). If you are at a Canadian institution, see your own PI for a CCRI number to get an account. If you are not at a Canadian instituion, when asked for a sponsors CCRI, use qbs-015-01 (CCRI of Carsten Krauss, e-mail: carsten.krauss@ualberta.ca). Make sure you email both Carsten and the head of the data-processing group when you have done so.

After receiving an account, log in to your account and run
```bash
echo $ID
```
Send this ID, your ComputeCanada username and an ssh key to Erming Pei (email: erming.pei@computecanada.ca) and the head of the data-processing group to get access to the snoprod account. Erming will add your ssh key to the authorized hosts and you should be able to ssh into the machine.

**Other Locations**

Occasionally, we will need to purchase time on another machine for large scale production. Depending on the machine, a person will be elected to be the contact for ensuring that production is occuring quickly and correctly. For example, we have purchased time on the Savio cluster at Berkeley which can take many months to get an account on. In this case, a person at Berkeley with an account already has been to setup and monitor jobs on the Savio cluster. We also have access to a small cluster at the Univeristy of Alberta called Illume. Although this cluster only has 500 or so cores, the load on the cluster is only IceCube and SNO+. A local UofA person is needed to access the cluster which operates on a condor submission system.
