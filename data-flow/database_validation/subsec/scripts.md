---
layout: default
title: Scripts
parent: Database Validation
grand_parent: SNO+ data-flow manual
nav_order: 2
---

# Scripts

The following scripts are available in the `data-flow/gasp/validation` folder: 

* [manage_production_labels.py] script to add or remove labels to MC production sets
* [manage_processing_labels.py] script to add or remove labels to processing runs
* [validate_data_documents.py] script to check if database matches grid information
* [refresh_guids.py] script to check if database guid matches the grid guid
* [refresh_guids_by_version.py] script to check if database guid matches the grid guid
* [fix_incomplete_paths.py] script to replace "INCOMPLETE" in some data paths by the correct host path
* [calculate_request_statistics.py] script to calculate the CPU usage and storage needed for a completed request.
* [fix_unregistered.py] script to register all documents with "notRegistered" flags
* [storage_site_file_scan.py] script to flag and unregister the zdabs on certain grid storage site and remove their corresponding CouchDB references.
* [set_grid_permissions.py] script to update permissions on the lfn in order to keep everything group writeable
* [delete_old_data.py] script to delete old data for either processing, production or pure data files.
