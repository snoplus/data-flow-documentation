---
layout: default
title: make_production
nav_exclude: true
---

## make_production

This script is used whenever there is a new release of RAT and production modules need to be run. The new version uploads the new information file to couchDB, which you can find in data-processing, under proddata, in the production_information view.

Example usage:
```bash
python bin/make_production [-c config] [rat_dir] [output]
# Required options:
#  [-c config]: specify the path to the processing config (usually gasp/config/processing.cfg)
#  [rat_dir]: point to the directory holding the fixed release of RAT (if it is in cvmfs, then simply source the corresponding RAT environment and you can put $RATROOT)
#  [output]: e.g. data-flow/gasp/modules/production_information_5_3_2.py for RAT version 5.3.2
```
The output file will have an appearance like:
```bash
"""Auto-generated dictionary of production module information. 
"""
production_modules = { 
  "PartialScintAc228_Scint": {"event_mode": "time", "path": "mac/production/partialscint/Ac228_scint.mac", "outputs": ["ntuple", "ratds"], "template": ['rate', 'z', 'day']},
  "PartialScintAc228_Water": {"event_mode": "time", "path": "mac/production/partialscint/Ac228_water.mac", "outputs": ["ntuple", "ratds"], "template": ['rate', 'z', 'day']}
}
```
Where fields are:
* event_mode: either `time`, `number`, or `run`
* path: relative path to macro within the fixed release of RAT
* outputs: list of outputs (skimmed from the types of output processors included in the file) - typically `ntuple`, `ratds`, `other_ntuple`, `other_ratds`
* other_suffix: OPTIONAL - OPTIONAL - Used to give a suffix to the “other_ntuple,ratds” files. **Needs to match the suffix specified in the RAT macro!**
* template: list (can be empty) of template strings (of format ${field}) within the macro
The file generated needs to be added to the git repository, but first should be checked to ensure all options are set as desired. For example, some new macros (especially if they do not use a generator with a rate, e.g. the solar neutrino macros or N16) may not have the event_mode set correctly.

Note that there is no automated way to generate the processing_information_x_x_x.py files, as these have a larger set of possible outputs and run dependent information as compared to the production modules. These need to be generated with the input of the different working groups and analyses.

