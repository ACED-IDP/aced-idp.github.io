# Project Workflow

Use case:  As a analyst, in order to share data with collaborators, I need a way to create a project, upload files and associate those files with metadata.  The system should be capable of adding files in an incremental manner. 

The following guide details the steps a data contributor must take to submit a project to the ACED data commons.

> In a Gen3 data commons, a semantic distinction is made between two types of data: “data files” and “metadata”. [more](https://gen3.org/resources/user/dictionary/#understanding-data-representation-in-gen3)

A “data file” could be information like tabulated data values in a spreadsheet or a fastq/bam file containing DNA sequences. The contents of the file are not exposed to the API as queryable properties, so the file must be downloaded to view its content.

“Metadata” are variables that help to organize or convey additional information about corresponding data files so that they can be queried via the Gen3 data commons’ API or viewed in the Gen3 data commons’ data exploration tool. In a Gen3 data dictionary, variable names are termed “properties”, and data contributors provide the values for these pre-defined properties in their data submissions.

For the ACED data commons, we have created a data dictionary based on the FHIR data standard. The data dictionary is available [here](https://github.com/bmeg/iceberg-schema-tools)

### Installing the Requirements


## Dependencies

* Credentials: `~/.gen3/credentials.json` [identity file](https://gen3.org/resources/user/using-api/#credentials-to-send-api-requests) from the [portal](https://aced-training.compbio.ohsu.edu/identity)
  * Download the access key from the portal and save it in the standard location `~/.gen3/credentials.json`
![image](https://gen3.org/resources/user/using-api/API_key_profile_page.png)
  
* Upload and download data files: A configured [gen3-client](https://gen3.org/resources/user/gen3-client/#2-configure-a-profile-with-credentials) for file uploads and downloads
  * Download and configure the gen3-client executable 


### Testing the configuration

The command `gen3_util ping` will confirm that the access key and gen3-client have been configured correctly

```commandline
$gen3_util ping 
msg: Configuration OK, access key found, gen3-client found, gen3-client profile found 'aced'
endpoint: https://aced-idp.org
username: me@myinstitution.edu

```

## Usage:

`gen3_util  [OPTIONS] COMMAND [ARGS]...`

  The following options and environmental variables are synonymous, you may set them as environmental variables or pass them as parameters to the command line.


| option                  | environment           | comment       
|-------------------------|-----------------------|---------------|
| --restricted_project_id | RESTRICTED_PROJECT_ID | authorization |
| --project_id            | PROJECT_ID            | authorization |
| --observation_id        | OBSERVATION_ID        | meta data     |
| --patient_id            | PATIENT_ID            | meta data     |
| --specimen_id           | SPECIMEN_ID           | meta data     |
| --task_id               | TASK_ID               | meta data     |
| --profile | GEN3_PROFILE          | gen3-client profile |


e.g.

    `--observation_id observation-1` or `export OBSERVATION_ID=observation-1`

The following parameters may be used to control system wide behavior:

| environment | comment                                                                | default |
|-------------|------------------------------------------------------------------------|---------|
| GEN3_UTIL_CONFIG | Path to config file. | None    |
| GEN3_UTIL_FORMAT | Output format. | yaml    |
| GEN3_UTIL_STATE_DIR | Path for logs and state information.  | ~/.gen3/gen3_util        |
| GEN3_API_KEY | location of credentials.json file. | ~/.gen3/credentials.json|

## Examples

> In a Gen3 Data Commons, programs and projects are two administrative nodes in the graph database that serve as the most upstream nodes. A program must be created first, followed by a project. Any subsequent data submission and data access, along with control of access to data, is done through the project scope.
> [more](https://gen3.org/resources/operator/#6-programs-and-projects)

For the following examples, we will use the `test` program, please use the `gen3_util projects ls` command to verify what programs you have access to.



## Create a project in authorization system


```text
export PROJECT_ID=test-myproject  # change this value to your program-project

# request a new project
gen3_util projects new

# Note: before any project is created, the request must be approved. 
# A user with appropriate authority signs requests for the current project
gen3_util access sign  # optionally, use the `--username nancy@example.com` to limit the approvals to a specific user


```

# Use case: upload files and associate them with a study


## add files to the manifest

```text
# find all files under directory tests/fixtures/add_files_to_study
# note we use xargs' `-P 9` argument to run commands in parallel
find tests/fixtures/add_files_to_study -type f  | xargs -P 9 -I PATH gen3_util files manifest put PATH

```

## Check the file names in the upload manifest

```text
gen3_util files manifest ls | grep file_name
#  file_name: ...
```

## Reset the manifest

You can use the `gen3_util files manifest ls` command to view the current manifest.
If you need to remove files from the manifest, you can use the `gen3_util files manifest rm` command.
Note that this command will not remove the files from the bucket, it will only remove them from the manifest.



## Upload and index the files

```text
gen3_util files manifest upload

```

## Create basic, minimal metadata for the project

```text
gen3_util meta create indexd /tmp/$PROJECT_ID
```

### Optional: edit or validate the metadata

The `meta create` command will create minimal FHIR resources for the files and identifiers.  You may wish to enhance the meta data by populating additional attributes.
You may also provide your own FHIR resource files.   Refer to https://aced-training.compbio.ohsu.edu/DD for additional information.

```text
# files created from the previous step
ls -1 /tmp/$PROJECT_ID
DocumentReference.ndjson
ResearchStudy.ndjson
```


## Publish the project metadata to the portal

```text
# copy the metadata to the bucket and publish the metadata to the portal
gen3_util meta publish  /tmp/$PROJECT_ID

```


## View in portal
<img alt="image" src="https://github.com/ACED-IDP/data_model/assets/47808/133ef835-63d6-473e-80ad-9c4e0de62651">


# Use case: upload files and associate them with a patient, specimen or observation

## Create a project in authorization system

> see above

## Add file to the project, we assign a patient identifier to the file

> Note: the identifiers used are arbitrary, they are not validated.
> * a unique alphanumeric identifier for that record across the whole project and is specified by the data submitter
> * **No PHI values should be used.**
> For more see: https://build.fhir.org/datatypes.html#Identifier
>
```commandline

export PROJECT_ID=test-myproject

gen3_util files manifest put \
    --patient_id patient-1 \
    tests/fixtures/add_files_to_study/file-1.txt
```

## Add file to the project, note we assign a patient identifier and specimen identifier to the file

```commandline
gen3_util files manifest put \
    --patient_id patient-1 \
    --specimen_id specimen-1 \
    tests/fixtures/add_files_to_study/file-2.csv
```

## Add file to the project, note we assign a patient identifier and observation identifier to the file

```commandline
gen3_util files manifest put \
    --patient_id patient-1 \
    --observation_id observation-1 \
    tests/fixtures/add_files_to_study/sub-dir/file-3.pdf
```

## Add file to the project, note we assign a patient identifier, specimen identifier and a task identifier to the file

```commandline

gen3_util files manifest put \
    --patient_id patient-1 \
    --observation_id observation-2 \
    --task_id task-1 \
    tests/fixtures/add_files_to_study/sub-dir/file-4.tsv

```

## upload and index the files

```text
gen3_util files manifest upload

```


## Create project metadata

```text
# create basic, minimal metadata for the project
gen3_util meta create indexd /tmp/$PROJECT_ID
```

### Optional: edit the metadata

```commandline
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

#### Check that your file and meta data have been successfully uploaded

<img width="1485" alt="image" src="https://github.com/ACED-IDP/gen3_util/assets/47808/d4d8c6bf-bb9a-49cf-affc-34daf78ce92c">


## Use case: analyze data, generate metadata and publish to the portal

* Assuming project exists, files have been uploaded and indexed, and metadata has been created and published.
* Query the system, produce file manifest. An example using the windmill ui:

![image](https://github.com/ACED-IDP/gen3_util/assets/47808/3aa0af6d-0112-4d6b-bb60-3749ef6b482f)

### Download the manifest using `gen3-client`:

#### Environment setup


```commandline
EXPORT PROJECT_ID=test-Alcoholism

# see https://gen3.org/resources/user/gen3-client/#2-configure-a-profile-with-credentials
EXPORT GEN3_CLIENT_PROFILE=<profile>
```
#### Download files using manifest

```commandline
gen3-client download-multiple --manifest  manifest-alcoholism.json  --profile $GEN3_CLIENT_PROFILE --filename-format original  --download-path /tmp/alcoholism --numparallel 9  --no-prompt
# ...
# 2023/10/02 12:57:36 14 files downloaded.

```

#### Process the downloaded files, using [pydicom](https://pydicom.github.io/) to extract file info

```commandline
find /tmp/alcoholism   -name '*.dcm' | xargs -n 1  pydicom show > analysis/dicom-info.txt

```

#### Add the resulting analysis file to the manifest

```commandline
gen3_util files manifest put analysis/dicom-info.txt
```

#### Check manifest

```commandline
gen3_util files manifest ls
# - object_id: ...
#  file_name: ...
```

#### Upload and index the file

```commandline
gen3_util files manifest upload --profile $GEN3_CLIENT_PROFILE
```

#### Create metadata for the analysis files. In this example, create minimal metadata using information uploaded into indexd.

```commandline
gen3_util meta create indexd meta/
```

#### Optionally edit or validate the metadata

```commandline
gen3_util meta validate meta
```

#### Publish the metadata to the portal

```commandline
gen3_util meta publish meta/
```

#### Check that your file and meta data have been successfully uploaded
![image](https://github.com/ACED-IDP/gen3_util/assets/47808/c705eb32-f636-42e2-992e-e076f1b28cb8)


### Restrict access to some files

Some use cases need to limit to some files to a subset of project members.

For example, given the example above, let's say we needed to restrict access to some files to only certain users.

First, we will set up a second project, by convention with a `_restricted` suffix.

```
gen3_util projects new --project_id=test-myproject
gen3_util projects new --project_id=test-myproject_restricted
```

Next, lets add a user to the primary project, but **not** the restricted project:

```commandline
gen3_util users add --username someone@example.com --project_id=test-myproject
```

Now, create a manifest using one of the methods above.

Then, use the `--restricted_project_id` option to the `gen3_util files manifest upload` command to add a second authorization requirement to those files. e.g.

```commandline
gen3_util files manifest upload --restricted_project_id test-myproject_restricted
```

In order to download files from that manifest, a user must read privileges to **both** test-myproject and test-myproject_restricted.
