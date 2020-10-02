---
layout: default
title: Scripts
parent: Database Validation
grand_parent: SNO+ Data-Flow Manual
nav_order: 2
---

# Scripts

The following scripts are available in the `data-flow/gasp/validation` folder: 

* [manage_production_labels.py](./scripts/manage_production_labels_py.md) script to add or remove labels to MC production sets
* [manage_processing_labels.py](./scripts/manage_processing_labels_py.md) script to add or remove labels to processing runs
* [validate_data_documents.py](./scripts/validate_data_documents_py.md) script to check if database matches grid information
* [refresh_guids.py](./scripts/refresh_guids_py.md) script to check if database guid matches the grid guid
* [refresh_guids_by_version.py](./scripts/refresh_guids_by_version_py.md) script to check if database guid matches the grid guid
* [fix_incomplete_paths.py](./scripts/fix_incomplete_paths_py.md) script to replace "INCOMPLETE" in some data paths by the correct host path
* [calculate_request_statistics.py](./scripts/calculate_request_statistics_py.md) script to calculate the CPU usage and storage needed for a completed request.
* [fix_unregistered.py](./scripts/fix_unregistered_py.md) script to register all documents with "notRegistered" flags
* [storage_site_file_scan.py](./scripts/storage_site_file_scan_py.md) script to flag and unregister the zdabs on certain grid storage site and remove their corresponding CouchDB references.
* [set_grid_permissions.py](./scripts/set_grid_permissions_py.md) script to update permissions on the lfn in order to keep everything group writeable
* [delete_old_data.py](./scripts/delete_old_data_py.md) script to delete old data for either processing, production or pure data files.
