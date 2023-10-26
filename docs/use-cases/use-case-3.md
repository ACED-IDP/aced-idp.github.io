---
title: Upload files and associate them with a study
---

> Note: the tools listed here are under development and may be subject to change

## Add files to the manifest

```sh
# find all files under directory tests/fixtures/add_files_to_study
# note we use xargs' `-P 9` argument to run commands in parallel
find tests/fixtures/add_files_to_study -type f  | xargs -P 9 -I PATH gen3_util files manifest put PATH

```

## Check the file names in the upload manifest

```sh
gen3_util files manifest ls | grep file_name
#  file_name: ...
```

## Reset the manifest

You can use the `gen3_util files manifest ls` command to view the current manifest.
If you need to remove files from the manifest, you can use the `gen3_util files manifest rm` command.
Note that this command will not remove the files from the bucket, it will only remove them from the manifest.

## Upload and index the files

```sh
gen3_util files manifest upload

```

## Create basic, minimal metadata for the project

```sh
gen3_util meta create /tmp/$PROJECT_ID
```

### Optional: edit or validate the metadata

The `meta create` command will create minimal FHIR resources for the files and identifiers. You may wish to enhance the meta data by populating additional attributes.
You may also provide your own FHIR resource files. Refer to https://aced-training.compbio.ohsu.edu/DD for additional information.

```sh
# files created from the previous step
ls -1 /tmp/$PROJECT_ID
DocumentReference.ndjson
ResearchStudy.ndjson
```

## Publish the project metadata to the portal

```sh
# copy the metadata to the bucket and publish the metadata to the portal
gen3_util meta publish  /tmp/$PROJECT_ID
```

## View in portal

<a href="https://aced-idp.org/explorer">![Gen3 File Explorer](https://github.com/ACED-IDP/gen3_util/assets/47808/d4d8c6bf-bb9a-49cf-affc-34daf78ce92c)</a>
