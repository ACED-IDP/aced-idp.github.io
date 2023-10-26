---
title: Upload files and associate them with a patient, specimen or observation
---

> Note: the tools listed here are under development and may be subject to change

## Create a project in authorization system

> see above

## Add file to the project, we assign a patient identifier to the file

> Note: the identifiers used are arbitrary, they are not validated.
>
> - a unique alphanumeric identifier for that record across the whole project and is specified by the data submitter
> - **No PHI values should be used.**
>   For more see: https://build.fhir.org/datatypes.html#Identifier

```sh

export PROJECT_ID=test-myproject

gen3_util files manifest put \
    --patient_id patient-1 \
    tests/fixtures/add_files_to_study/file-1.txt
```

## Add file to the project, note we assign a patient identifier and specimen identifier to the file

```sh
gen3_util files manifest put \
    --patient_id patient-1 \
    --specimen_id specimen-1 \
    tests/fixtures/add_files_to_study/file-2.csv
```

## Add file to the project, note we assign a patient identifier and observation identifier to the file

```sh
gen3_util files manifest put \
    --patient_id patient-1 \
    --observation_id observation-1 \
    tests/fixtures/add_files_to_study/sub-dir/file-3.pdf
```

## Add file to the project, note we assign a patient identifier, specimen identifier and a task identifier to the file

```sh

gen3_util files manifest put \
    --patient_id patient-1 \
    --observation_id observation-2 \
    --task_id task-1 \
    tests/fixtures/add_files_to_study/sub-dir/file-4.tsv

```

## Upload and index the files

```text
gen3_util files manifest upload

```

## Create project metadata

```text
# create basic, minimal metadata for the project
gen3_util meta create /tmp/$PROJECT_ID
```

### Optional: edit the metadata

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

## Publish the project metadata to the portal

```text
# copy the metadata to the bucket and publish the metadata to the portal
gen3_util meta publish  /tmp/$PROJECT_ID

```

## Check that your file and meta data have been successfully uploaded

<img width="1485" alt="image" src="https://github.com/ACED-IDP/gen3_util/assets/47808/d4d8c6bf-bb9a-49cf-affc-34daf78ce92c">
