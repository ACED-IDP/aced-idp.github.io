
# Quickstart Guide

## About

g3t is the command line tool used to interact with the ACED-IDP platform. The following tutorial outlines two different use cases for data contributors: uploading data to the platform and downloading data from the platform. 

## Requirements

Please ensure you have completed the following set up from the [Requirements](/requirements) page:

* installed gen3-client
* configured a profile with credentials
* installed gen3-tracker


To confirm all dependencies are set up as expected, run

```sh
g3t --profile <your_g3t_profile_name> ping
```

you should get a message like this
> msg: 'Configuration OK: Connected using profile:production'
endpoint: https://aced-idp.org
username: someone@example.com

## General Usage

`g3t  [OPTIONS] COMMAND [ARGS]...`

The following options and environmental variables are synonymous, please either set them as environmental variables or pass them as parameters to the command line.

| option       | environment     | comment             | example         |
|--------------|-----------------| ------------------- |-----------------|
| --project_id | G3T_PROJECT_ID  | authorization       | aced-myproject  |
| --profile    | G3T_PROFILE     | gen3-client profile | production      |
| --format     | G3T_FORMAT      | Output format.      | default: yaml   |

For example, a `ping` command using the `aced` profile would be

```sh
g3t --profile aced ping
```

Alternatively, to set the environmental variables using the `EXPORT` keyword:

```sh
export G3T_PROJECT_ID=aced-myproject
```

Many g3t commands behave similarly to git commands so users can not only upload files but also track their metadata.

For the following examples, we will use the `aced` program with a project called `myproject` and a `aced` g3t profile.


## 1. Upload Data to an Approved Project on ACED IDP

The first use case we will cover outlines how to add data to new project that you have access to on ACED-IDP.

!!!note
    Please ensure you have created a G3T_PROFILE environment variable, as the rest of the tutorial will assume you have done so.

### Check project permissions

Then, check what projects you have access to using the command

```sh
g3t projects ls
```

Ensure that you have permissions to edit `aced-myproject`. If not, please contact a system adminstrator to get the correct permissions.

### Initialize a new project

Then, initialize the new project in a new directory

```
mkdir aced-myproject
cd aced-myproject
g3t init aced-myproject
```

* Similar to `git init`, this command creates a new project in the current directory
* Make sure to specify a profile as an environment variable or as a flag in the command line
* The id is `aced-myproject`; the program name is `aced`, the project name is `myproject`
* `Program name` is predefined by the institution
* `Project name` must be unique within the server, be alphanumeric, and contain no spaces
* For more information, see [creating a project](creating-project.md)

### Add file(s)

```
g3t add folder/file.tsv
g3t add folder/file2.tsv
```

* For each data file, this command will record an entry in the `MANIFEST` directory.
* Optionally you can tag each file with patient, specimen or other identifiers. See `g3t add --help` for more information.
* Note: `g3t add`, as opposed to git add, creates entries for data files rather than just staging files for a g3t commit.  Your data files remain in place and are not checked in. 
* For more information, see [adding data](upload.md)

### Create metadata

The `g3t meta init` command will take care of creating FHIR compliant data for you.

```
g3t meta init
```

* This command will create metadata files in the META directory. At a minimum, the directory will contain:

| file                     | contents                   |
|--------------------------|----------------------------|
| ResearchStudy.ndjson     | Description of the project |
| DocumentReference.ndjson | File information           |

Additional metadata files for patient, specimen, and other entities will be generated based on options provided to the `add` command.

| file                   | contents               |
|------------------------|------------------------|
| Patient.ndjson         | Patient information    |
| ResearchSubject.ndjson | Enrollment information |
| Specimen.ndjson        | Sample information     |


> Note: `g3t meta init` command is a convenience method to generate the required metadata associated with the files. For your particular use case, you may also want to supply your own FHIR data.  

### Check that the metadata is valid

```
g3t meta validate
```

The system will print summary counts and an informative messages if the metadata is invalid.


### Check that the expected files are queued for upload
```
g3t status
```
- This command will determine what files have not been checked in yet.

### Commit files
```
g3t commit -m "Adding files"
```
- See `g3t commit --help` for more info

### Push to ACED-IDP
```
g3t push
```

Notes:

* This command launches a job to upload project data to the specified data platform.
* a push checks that the metadata is valid and all files are committed before pushing.
* a push will fail if a data file had already been uploaded. To force an upload, use the `--overwrite` option.
* If a job fails, look for more information in the logs directory. This directory stores rolling logs, where each line is a JSON representing a single submission attempt.

There are several other options available for the `push` command for specialized use cases. See `g3t push --help` for more information.

### View the upload status (pending or complete)
```
g3t status
```

Check that your data was uploaded successfully by viewing the Exploration page on [aced-idp.org](https://aced-idp.org)!

## 2. Download Data from a Project on ACED-IDP

To download just the metadata for a project, use the clone command.

```commandline
g3t clone [OPTIONS] PROJECT_ID 
```

- The clone command will download the `MANIFEST/` and `META/` directories associated with the project into its own directory. 
- The `PROJECT_ID` is the name of the project you wish to download. 

To retrieve the actual data files associated with the file metadata, use the pull command.

```commandline
mkdir aced-myproject
g3t init myproject
g3t pull
```

- The pull command will retrieve the actual data files associated with the manifest.


To download only a subset of files, refer to the downloads [page](https://aced-idp.github.io/workflows/portal-download/). For more information on other commands or use cases, see the Use Cases & Workflows section.
