---
title: Upload Files
---

{% include '/note.md' %}

Uploading files to a project is a two-part process:

1. Creating and uploading files to a manifest
2. Creating and publishing file metadata to the browser

This page will guide you through both steps using the `gen3_util` tool. 

## Creating and Uploading Manifest

First, set the `PROJECT_ID` environmental variable to that of your program and project (e.g. `aced-myproject`):

```sh
export PROJECT_ID=aced-myproject
```

### Upload a Single File

To upload a single file to the manifest, run:

```sh
gen3_util files manifest put example-file.txt
```

### Upload Multiple Files

To upload multiple files to the manifest, use the `find` and `xargs` commands to send files the `gen3_util`. 

For example, to add all files in the `example-directory/` directory, run:

```sh
find example-directory -type f  | xargs -P 0 -I PATH gen3_util files manifest put PATH
```

Note that we use `xargs` `-P 0` argument to run commands in parallel, greatly reducing the amount of time to add many files to a manifest.

### Verify the Manifest

```sh
gen3_util files manifest ls | grep file_name
```
If incorrect, then delete the manifest `gen3_util files manifest rm` and then re-add all files using the `gen3_util files manifest put` command shown previously.

### Upload the Manifest

```sh
gen3_util files manifest upload
```

This command will upload all files defined in the newly create manifest to the S3 storage endpoints associated with the project.

## Creating and Uploading Metadata

### Create Metadata

Create basic, minimal metadata for the project:

```sh
gen3_util meta create /tmp/$PROJECT_ID
```

### Optional: Edit the Metadata

```sh
ls -1 /tmp/$PROJECT_ID
DocumentReference.ndjson
Observation.ndjson
Patient.ndjson
ResearchStudy.ndjson
ResearchSubject.ndjson
Specimen.ndjson
Task.ndjson
```

### Publish the Metadata

```text
# copy the metadata to the bucket and publish the metadata to the portal
gen3_util meta publish /tmp/$PROJECT_ID
```

## View the Files

This final step uploads the metadata associated with the project and makes the files visible on the [Explorer page](https://aced-idp.org/explorer).

<a href="https://aced-idp.org/explorer">![Gen3 File Explorer](./explorer.png)</a>
