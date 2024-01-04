---
title: Upload Files
---

{% include '/note.md' %}

Uploading files to a project is a two-part process:

1. Creating and uploading files to a manifest
2. Creating and publishing file metadata to the browser

This page will guide you through both steps using the `gen3_util` tool. 

## Creating and Uploading Manifest

Think of a 'manifest' for file uploads as a detailed packing list for items being sent in a shipment. 
It's like a comprehensive index or catalog that describes all the files being uploaded, providing essential information about each file in a structured format.
For each file in the manifest, the following information is collected:

* Identification and Details
  * `file_name` - the relative full path name of the file [required]
  * `size` - the size of the file in bytes [filled automatically]
  * `md5` - the md5 checksum of the file [filled automatically]
  * `object_id` - the object id of the file in indexd [system generated] 
  * `mime_type` - the mime type of the file [filled automatically] 
  * `remote_path` - the path to the file in when downloaded from the bucket [optional]
  * meta-data - any identifiers that are associated with the file
    * `project_id` - the project id of the file [required]
    * `specimen_id` - the specimen id of the file [optional]
    * `patient_id` - the patient id of the file [optional]
    * `task_id` - the task id of the file [optional]
    * `observation_id` - the observation id of the file [optional]



A single manifest can exist for a project at any one time.

Set the `PROJECT_ID` environmental variable to that of your program and project (e.g. `aced-myproject`):

```sh
export PROJECT_ID=aced-myproject
```

### Add a single file to the project

To upload a single file to the manifest, run:

```sh
gen3_util files add example-file.txt
```

### Add multiple files to the project

To upload multiple files to the manifest, use the `find` and `xargs` commands to send files the `gen3_util`. 

For example, to add all files in the `example-directory/` directory, run:

```sh
find example-directory -type f  | xargs -P 0 -I PATH gen3_util files add PATH
```

Note that we use `xargs` `-P 0` argument to run commands in parallel, greatly reducing the amount of time to add many files to a manifest.

### Verify the files were added to the project

```sh
gen3_util files status | grep file_name
```

### Removing files from the project

```sh
gen3_util files rm --object_id xxxx
# use the --local flag to remove the file from the local system's submission manifest only
```


### Upload the project to the bucket and portal

```sh
gen3_util files push
```

This command will upload all files defined in the newly create manifest to the S3 storage endpoints associated with the project.

By default, minimum metadata is created for each file, based on the project id and other identifiers.  You can override this behavior with the `--metadata` flag. 

```commandline
gen3_util files manifest upload --help
Usage: gen3_util files manifest upload [OPTIONS]

  Upload to index and project bucket.  Uses local manifest, or manifest_path.

Options:
  --project_id TEXT             Gen3 program-project authorization
  --restricted_project_id TEXT  Gen3 program-project, additional authorization
  --upload-path TEXT            gen3-client upload path  [default: .]
  --duplicate_check             Update files records  [default: False]
  --manifest_path TEXT          Provide your own manifest file.
  --meta_data                   Generate and submit metadata.  [default: True]
  --wait                        Wait for metadata completion.  [default: False]

```

See the  <a href="/workflows/metadata/">metadata workflow section</a> for more information on how to create and upload metadata.

