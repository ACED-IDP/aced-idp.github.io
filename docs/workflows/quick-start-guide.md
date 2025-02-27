
# Quickstart Guide

{% include '/note.md' %}

## About

gen3 tracker, or g3t, is a command line tool for the ACED-IDP platform. It provides a set of utilities for users to upload, download, and keep track of your data using the platform. The following tutorial will walk you through the steps for two different use cases:

1. Uploading files for a new project to the platform
2. Downloading an existing project from the platform

## Requirements

Please ensure you have completed the following set up from the [Requirements](/requirements) page:

1. installed gen3-client
2. configured a gen3-client profile with credentials
3. installed gen3-tracker


To confirm all dependencies are set up as expected, run

```sh
g3t --profile <your_g3t_profile_name> ping
```

You should get a message like this
> msg: 'Configuration OK: Connected using profile:aced'
endpoint: https://aced-idp.org
username: someone@example.com

along with the set of projects you have been provided access to.

## General Usage

`g3t [OPTIONS] COMMAND [ARGS]...`

We have built g3t on git, so many commands behave similarly to git with some key differences.These differences will be outlined for each step in the submission process.

## 1. Upload Data to a Newly Approved Project

The first use case we will cover is how to add data to a new project on the ACED-IDP.

!!!note
    For the following examples, we will use the `aced` program with a project called `myproject` and a `aced` g3t profile.

### Check Project Permissions

To start, check what projects you have access to using the command

```sh
g3t projects ls
```

Check that you have permissions to edit `aced-myproject`. This is what allows you to push up data to the platform. If not, please contact a system adminstrator to get the correct permissions.

### Specify a gen3 Profile

For most g3t commands, you will need to specify the gen3-client profile you want to use. This ensure that you are uploading to the right platform with the right credentials. There are two ways to set your profile...

To set profile using an environmental variable:
```sh
export G3T_PROFILE=aced
```

To pass the profile as a flag to the `ping` command for example:

```sh
g3t --profile aced ping
```

For the rest of the tutorial, we will assume you have exported a `G3T_PROFILE` environment variable.

### Initialize a new project

To first, initialize your new project locally, you can use `g3t init`

```bash
mkdir aced-myproject
cd aced-myproject
g3t init aced-myproject
```

* Similar to `git init`, this command creates a new project in the current directory
* Within the project, a `MANIFEST/` directory is used to store file metadata entries a `META/` directory is created to house metadata converted into the [FHIR](https://hl7.org/fhir/) standard
* The project id is `aced-myproject` made from the program name `aced` and project name `myproject`. Specifically,
    * **Program name:** is predefined by the institution
    * **Project name:** must be unique within the server, be alphanumeric, and contain no spaces or hypens
* For more information, see [creating a project](creating-project.md)

### Add file(s)

Once your project is initialized, you can add files to the project. For example, if you have tsv files in a local directory called "folder/", add them using `g3t add`

```bash
g3t add folder/file.tsv --patient patient_1
g3t add folder/file2.tsv --patient patient_2
```

* Each `g3t add` creates a metadata entry for the specified data file, automatically calculating metadata like the file's md5sum, type, date modified, size, and path.
   This entry is written to a  `.dvc` files in the `MANIFEST` directory, where the dvc file path mirrors the original file path
   * **Example:** `folder/file.tsv` writes to `MANIFEST/folder/file.tsv`
   * Just as a ship's manifest is an inventory of its cargo, the `MANIFEST/` directory is an inventory for all your file details
* Using the patient flag is one example of creating additional metadata about the file, in this case associating each file with a specified patient identifier.
* `g3t add` varies from `git add`, as the `.dvc` file is what gets staged rather than the potentially large data file
* Multiple files can be added as the same time using wildcards wrapped in quotes, for example `g3t add "*.csv"`.
* For more information on usage, such as adding entries for remote files or associating files with a other entities like a specimen, see [adding data](upload.md)
  <!-- TODO: update adding data -->


### Create metadata

Now that your files have been staged with metadata entries, you can create FHIR-compliant metadata using the `g3t meta init` command

```bash
g3t meta init
```

* Using the file metadata entries created by the `g3t add` command, `g3t meta init` creates FHIR-compliant metadata files in the `META/` directory, where ecah file corresponds to a [FHIR resource](https://build.fhir.org/resourcelist.html). At a minimum, the directory will contain:

| File                     | Contents                   |
|--------------------------|----------------------------|
| ResearchStudy.ndjson     | Description of the project |
| DocumentReference.ndjson | File information           |

- Additional metadata files for patient, specimen, and other entities will be generated based on options provided to the `add` command.

| File                   | Contents               |
|------------------------|------------------------|
| Patient.ndjson         | Patient information    |
| ResearchSubject.ndjson | Enrollment information |
| Specimen.ndjson        | Sample information     |


- `meta init` focuses on creating metadata pertaining to the files you add. For your particular use case, you may also want to supply your own FHIR data, see [adding metadata](metadata.md)

### Check that the metadata is valid

To ensure that the FHIR data has been properly formatted, you can call `g3t meta validate`.

```bash
g3t meta validate
```

-The system will print summary counts and informative messages if the metadata is invalid.


### Check that the expected files are queued for upload

You can double check that all of our files have been staged before commiting with `g3t status`

```bash
g3t status
```

### Commit files

With our checks complete, you can commit the metadata we created using `g3t commit`.

```bash
g3t commit -m "adding tsv metadata"
```
<!-- TODO this is not enough -->
- Like git, this command bundles the staged files into a single set of changes.
  - The `-m` flag provides a commit message detailingt the changes 
  - If the commit is successful, you will see the a summary of the changes
- As a reminder, the files that are committed to git are the FHIR metadata in META/ and the dvc entries in MANIFEST/, not the data files themselves
- See `g3t commit --help` for more info

### Push to ACED-IDP

To submit the files and metadata to the data platform, we can use `g3t push`

```bash
g3t push
```

<!-- TODO: create summary from push docs -->
* This command launches a job to upload project data to the specified data platform.
* A push checks that the metadata is valid and all files are committed before pushing.
* A push will fail if a data file had already been uploaded. To force an upload, use the `--overwrite` option.
* Make sure to monitor the logs generated by the command
    * If a job is successful, you will get a green success message.
    * If a job fails, you will get a red error message: look for more information in the specified logs directory.
    * The logs directory stores rolling logs, where each line is a JSON representing a single submission attempt.
* For other publishing options and specialized use cases, see [publishing a project](commit-push.md)

### View the Data on the Platform

Congratulations, you have submitted data to the platform! To check that your data was uploaded,login and navigate to the Exploration page on [aced-idp.org](https://aced-idp.org)!

## 2. Download Data from a Project on ACED-IDP

Sometimes you might have access to a data project that is already in progress and want the most recent version of data on the platform. To download the metadata for an existing project, use the `g3t clone` command.

```commandline
g3t clone aced-myproject
```

- The clone command will download the metadata associated with the project into a new directory
- Specifically, it downloads the metadata `.dvc` entries in `MANIFEST/` and the FHIR-compliant metadata in `META/`
- The `PROJECT_ID` is the name of the project you wish to download. 

To retrieve the actual data files associated with the file metadata, use the pull command.

```commandline
mkdir aced-myproject
cd aced-myproject
g3t init aced-myproject
g3t pull
```

- The pull command will retrieve the actual data files associated with the metadata.
- Alternatively, 


To download only a subset of files, refer to the downloads [page](https://aced-idp.github.io/workflows/portal-download/). For more information on other commands or use cases, see the Use Cases & Workflows section.
