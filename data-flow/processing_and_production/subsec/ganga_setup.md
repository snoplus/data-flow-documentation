---
layout: default
title: Ganga setup
parent: Processing & Production
grand_parent: SNO+ data-flow manual
nav_order: 5
---

# Ganga setup
All processing and production clients should:
* Access Ganga via CVMFS (the data-flow/env.sh will set this up).
* Contain a .gangarc_[xxx] and gangadir_[xxx] per client required, for example a node that runs both production and processing jobs will need to use two setups plus a user setup for testing:
  * .gangarc_production and gangadir_production (production).
  * .gangarc_processing and gangadir_processing (processing).
  * .gangarc and gangadir (user testing).
  * The gangarc contains a field [Configuration]: gangadir to set the correct gangadir.
  * Running `ganga –config=$HOME/.gangarc_[xxx]` will select the correct config.
* The following fields in each .gangarc must be set appropriately:
  * [Configuration]: gangadir (see entry on gangarc and gangadir).
  * If submitting to a batch system:
    * [Configuration]:Batch (set to LFC, PBS, SLURM, or SGE depending on system, no others supported).
    * Specific Batch settings in [LFC]/[PBS]/[SGE]/[SLURM] (e.g. regex pattern for submission return string to extract job ID).
    * Specific Batch defaults in [defaults_LFC]/[defaults_PBS]/[defaults_SGE]/[defaults_SLURM] (e.g. queue to submit to).
    * A SNO+ specific backend has been developed called WestGrid to generalize the other batch systems. Users would have to change the [WestGrid] and [defaults_WestGrid] configurations.
    * As the CONDOR batch system is very different from the others, it has it's own backend called Illume.
  * If submitting to Dirac:
    * Configuration: RUNTIME_PATH (needs to contain GangaDirac if submitting via Dirac).
    * Dirac: DiracEnvFile (the env file needs to contain a dirac proxy for X509_USER_PROXY.
    * Dirac: Timeout (set to a smaller value than the default, e.g. 30)
  * Note that other defaults are preset in data-flow/gasp/GangaSNOplus/GASP.ini.

Note if you are running ganga with an active grid proxy, ganga will use that proxy and its associated roles (e.g. production) to check all jobs currently in the jobs repository. This causes problems if you have both production and lcgadmin jobs running, and will likely cause ganga to think that the jobs have failed. To solve for this, the easiest solution is to use separate jobs repositories for jobs that will be/were submitted with different roles. Hence the separate gangarc and gangadir folders specificied above. To run ganga with a specific configuration:
```bash
ganga --config=/path/to/config [script, options, etc]
```
More information can be found at the [Ganga](https://ganga.readthedocs.io/en/latest/UserGuide/InstallAndBasicUsage.html) documentation page.
