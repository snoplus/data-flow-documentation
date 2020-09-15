---
layout: default
title: Scripts
parent: Database Validation
grand_parent: SNO+ data-flow manual
nav_order: 2
---

# Scripts

The following scripts are available in the `data-flow/gasp/validation` folder: 

* [manage_production_labels.py](./manage_production_labels_py.md) script to add or remove labels to MC production sets
* [manage_processing_labels.py](./manage_processing_labels_py.md) script to add or remove labels to processing runs
* [validate_data_documents.py](./validate_data_documents_py.md) script to check if database matches grid information
* [refresh_guids.py](./refresh_guids_py.md) script to check if database guid matches the grid guid
* [refresh_guids_by_version.py](./refresh_guids_by_version_py.md) script to check if database guid matches the grid guid
* [fix_incomplete_paths.py](./fix_incomplete_paths_py.md) script to replace "INCOMPLETE" in some data paths by the correct host path
* [calculate_request_statistics.py](./calculate_request_statistics_py.md) script to calculate the CPU usage and storage needed for a completed request.
* [fix_unregistered.py](./fix_unregistered_py.md) script to register all documents with "notRegistered" flags
* [storage_site_file_scan.py](./storage_site_file_scan_py.md) script to flag and unregister the zdabs on certain grid storage site and remove their corresponding CouchDB references.
* [set_grid_permissions.py](./set_grid_permissions_py.md) script to update permissions on the lfn in order to keep everything group writeable
* [delete_old_data.py](./delete_old_data_py.md) script to delete old data for either processing, production or pure data files.
