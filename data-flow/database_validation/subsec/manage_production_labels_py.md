---
layout: default
title: manage_production_labels.py
nav_exclude: true
---

## manage_production_labels.py

This script is used to add or remove labels from the job and data documents of a particular set of MC production. It requires the RAT version and module to be specified as inputs. Example usage:

```bash
python bin/manage_production_labels -a [action] -m [module] -v [rat_version] -n [database] -s [server] label_name
# Required arguments:
#  -a [action]: Action to perform, only "add" or "remove"
#  -m [module]: Module of the production set which labels need to be updated, 'ALL' selects all modules of this rat version'
#  -v [rat_version]: RAT version of the production set which labels need to be updated
#  --start [run number]: First run to label
#  --start [run number]: Last run to label (inclusive)
#  -l [txt file]: Run list (format = module,run,pass)
#  label_name: Name of the label 
# Optional arguments:
#  -n [database]: database of interest, default: data-production
#  -s [server]: database server of interest, default: https://couch.snopl.us
#  -c [config]: CouchDB configuration file,
```

To perform one of the two add/remove actions, one must specify either:

- A rat version, a module name and a run range.
Example: python manage_production_labels.py -m WaterBi214_Exwater -v 6.1.1 –start 101000 –end 103000 -a add my_label

- A list in the form of a text file.
Example: python manage_production_labels.py -l testlist.txt -a add my_label

This text file must have the format: Module Run Pass.
Example:

WaterBi214_Exwater 101967 7
WaterTl208_Exwater 106538 2

written in a file like runlist.txt.

Replacing the module name by ALL (not in a run list) will select all modules produced with this RAT version (note: this might take a long time since the numbers of different modules is usually high).

Note that, once the label list of the job and data documents can contain several different labels, when the action is set to "remove", only the label name specified as input will be removed. For instance, to remove the "Water_Test" label of production set with module "WaterBi214_Pmt" produced with RAT 6.1.1, one would have to type:

python bin/manage_production_labels -a remove -m WaterBi214_Pmt -v 6.1.1 Water_Test

Similarly, adding a label would be performed by:

python bin/manage_production_labels -a add -m WaterBi214_Pmt -v 6.1.1 Water_Test

This would add the "Water_Test" label to the list in the job and data documents. If the label is already present, it won't be added a second time.

Note: You do not have to provide -n, -s, or -c arguments in order to run the script but if you provide -c (config file) you do not need to provide -n and -s, and vice-versa. 
