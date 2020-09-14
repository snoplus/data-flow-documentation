---
layout: default
title: Generating info files
nav_exclude: true
---

## Generating processing and production information files

Information on how to find and run macros is held in data-flow/gasp/modules/production_information_x_y_z.py and data-flow/gasp/modules/processing_information_x_y_z.py where x, y, z are the RAT version (x.y.z with `.` replaced by `_`).

For production modules these files can be created semi-automatically:
```bash
python data-flow/gasp/bin/misc/make_production /path/to/rat/tag data-flow/gasp/modules/production_information_x_y_z.py
```
Some modules will have its event rate set internally(e.g. solar generator and reactor neutrino generator), therefore make_production won't correctly capture the macro type as time. In this case, an optional flag –internal_rate need to be given to make_production with the module name. The flag contains by default the solar generator and reactor neutrino generator. For example, if a new generator called "generator1" is created with an internal event rate, the following line should be used:
```bash
python data-flow/gasp/bin/misc/make_production /path/to/rat/tag data-flow/gasp/modules/production_information_x_y_z.py --internal_rate generator1
```
Particularly modules such as N16 or other Calibration runs may request a different run condition. If there are new folders with production macros then these may not be inserted into the information correctly. Check the code and the output files. Currently, the following directories and their sub-folders are added:
```bash
rat/mac/production/teloaded
rat/mac/production/scint
rat/mac/production/water
rat/mac/production/partial
```
The module naming format is as follows:
* Folder name included in module name (e.g. teloaded).
* Sub-folder(s) included in module name (e.g. teloaded/perylene) with the exception of calibration folders.
* Capitalise first letter of section and macro (e.g. teloaded/bi214.mac becomes TeloadedBi214, teloaded/perylene/bi214.mac becomes TeloadedPeryleneBi214).

More information is available in the scripts section.

For processing scripts, files must be created by hand. Note that the information file is only required if this version of RAT is to be used for processing. For example, processing_information_6_0_1.py contains:
```bash
from GangaSNOplus.Lib.Files import RATDBFile, RATDBAttachment, GridFile, PGSQLFile

processing_modules = {“Processing”: {“type”: “macro”,
                                       “level”: “sub_run”,
                                       “path”: “mac/processing/partial_water/processing.mac”,
                                       “ganga_outputs”: [],
                                       “outputs”: [“ratds”],
                                       “prerequisites”: [],
                                       “raw_processor”: True},
                      “DQCSS”: {“type”: “macro”,
                                  “level”: “run”,
                                  “path”: “mac/processing/partial_water/runLevelProcessing.mac”,
                                  “ganga_outputs”: [PGSQLFile(namePattern = “CSS_RESULT_*.ratdb”,
                                                                dbServer = “postgres://pgsql.snopl.us:5400”,
                                                                dbName = “ratdbtest”,
                                                                passDefault = -1,
                                                                version = 1),
                                                      PGSQLFile(namePattern = “DATAQUALITY_RECORDS*.ratdb”,
                                                                dbServer = “postgres://pgsql.snopl.us:5400”,
                                                                dbName = “ratdbtest”,
                                                                passDefault = -1,
                                                                version = 1)],
                                  “outputs”: [],
                                  “prerequisites”: [“Processing”],
                                  “raw_processor”: False}}

modules_for_run_type = {
    “Physics”: set([“Processing”, “DQCSS”]),
    “DeployedSource”: set([“Processing”, “DQCSS”]),
    “ExternalSource”: set([“Processing”, “DQCSS”]),
    # End of main run types
    “TELLIE”: set([“Processing”, “DQCSS”]),
    “SMELLIE”: set([“Processing”, “DQCSS”]),
    “AMELLIE”: set([“Processing”, “DQCSS”])
    }
```
The `processing_information` entries define how each module will be run. The different values are:
* “type”: either “macro” (run via RAT) or “script” (e.g. a bash script); almost all will be “macro”
* “level”: one of the job types for processing specified in the following section
* “path”: relative path within the release to the macro or script
* “ganga_outputs”: outputs that must be handled by the client, includes DB posts (anything not covered by the main “outputs” field)
* “outputs”: output types to expect (can be “ratds”, “ntuple”, “soc”, “other_ratds”, “other_ntuple”)
* “other_suffix”: OPTIONAL - Used to give a suffix to the “other_ntuple,ratds” files.**Needs to match the suffix specified in the RAT macro!**
* “prerequisites”: name(s) of modules required before this module can run
* “raw_processor”: does this processor run on raw (zdab) or processed (ROOT) data (true or false)?
The `modules_for_run_type` entries describe which modules will be added for a given run type (from RUN tables in RATDB).

### Job Types

There are various types of job that can be submitted depending on whether the module is production or processing.

For processing, the job type determines whether subfiles or entire runs are submitted and which inputs the job receives. Processing module types:
* “sub_run”: Creates one subfile per subjob (submitted separately to batch systems)
* “run”: Creates one subjob for the entire run. ROOT files are streamed over XrootD and macros must have template parameters to receive file paths.
* “run_zdab”: Creates one subjob for the entire run. ZDAB files are downloaded prior to execution and macros must have template parameters to receive file paths.
* “run_root”: Creates one subjob for the entire run. ROOT files are downloaded prior to execution and macros must have template parameters to receive file paths.

For production, the job type specifies the information that must be provided when the module is submitted. Production module types:
* “run”: Passes -r [runno] to RAT to simulate real run conditions
* “run_number”: Passes -n [runno] -N [num_ev] to RAT to simulate a number of events under real run conditions
* “time”: Passes -T to RAT to simulate a rate for a certain amount of time
* “number”: Passes -N to RAT to simulate a number of events
