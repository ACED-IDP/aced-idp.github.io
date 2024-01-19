---
title: Adding files
---

{% include '/note.md' %}

Uploading files to a project is a two-part process:

1. Adding files to a manifest
2. Adding file metadata that associates those files with subjects, samples and other metadata.

This page will guide you through adding files using the `g3t` tool. 

## Data directories

All data directories are **relative to the root of the project**. You have flexibility as to their names and hierarchy. However, they __must be__ relative to the root of the project. Here's a brief explanation of this convention:

* Portability: Relative paths make your project more portable, meaning that it can be moved to different locations or shared with others without causing issues with file references. This is particularly important in data engineering projects where datasets and files may be stored in different locations.
* Ease of Collaboration: When working on a data engineering project with multiple team members, using relative paths ensures that everyone can run the code without having to modify file paths based on their local directory structure. This promotes smoother collaboration.
* Consistency Across Environments: Data engineering projects often involve processing large datasets, and the code needs to run consistently across different environments (e.g., development, testing, production). Relative paths help maintain this consistency by allowing the code to reference files and directories relative to the project's root.

```
 g3t add --help
Usage: g3t add [OPTIONS] LOCAL_PATH

  Add file to the index.

  local_path: relative path to file on local file system

Options:
  --specimen_id TEXT     fhir specimen identifier
  --patient_id TEXT      fhir patient identifier
  --task_id TEXT         fhir task identifier
  --observation_id TEXT  fhir observation identifier
  --md5 TEXT             MD5 sum, if not provided, will be calculated before upload
```

## Migration of existing project data

If you have an existing project that you want to migrate using g3t, you can do so by following these steps:

* Create a new repository using `g3t init`. See [Creating a project](/workflows/creating-project/)
* Either move or copy your existing data files into the new repository.  Alternatively, symbolic links are supported.


## Add multiple files to the manifest

To upload multiple files to the manifest, use the `find` and `xargs` commands to send files the `g3t`. 

For example, to add all files in the `example-directory/` directory, run:

```sh
find example-directory -type f  | xargs -P 0 -I PATH g3t add PATH
```

Note that we use `xargs` `-P 0` argument to run commands in parallel, greatly reducing the amount of time to add many files to a manifest.

## Next steps


* See the  <a href="/workflows/status/">status command</a> for more information on how to verify the manifest.
* See the  <a href="/workflows/metadata/">metadata workflow section</a> for more information on how to create and upload metadata.

