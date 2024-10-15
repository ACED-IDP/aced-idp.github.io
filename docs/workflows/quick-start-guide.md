
# Quickstart Guide

## Dependencies

Please ensure you have the following dependencies installed:

* [gen3-client](../requirements/gen3-client.md)
* [gen3-tracker](../requirements/gen3-tracker.md)

## Usage

`g3t  [OPTIONS] COMMAND [ARGS]...`

The following options and environmental variables are synonymous, you may set them as environmental variables or pass them as parameters to the command line.

| option       | environment     | comment             | example         |
|--------------|-----------------| ------------------- |-----------------|
| --project_id | G3T_PROJECT_ID  | authorization       | aced-myproject  |
| --profile    | G3T_PROFILE     | gen3-client profile | production      |
| --format     | G3T_FORMAT      | Output format.      | default: yaml   |

For example, a `ping` command using the `aced` profile would be `g3t --profile aced ping`

Alternatively, to set the environmental variables using the `EXPORT` function e.g.:

```sh
export G3T_PROJECT_ID=aced-myproject
```


## 1. Upload Data (To an Existing Project on ACED IDP)
This section outlines how to add data to ACED IDP. Many `g3t` commands behave similarly to `git` commands

### Initialize a new project:

```
g3t init aced-myproject
```

* Similar to `git init`, this command initializes a new project in the current directory
* The id is `aced-myproject`; the program name is `aced`, the project name is `myproject`
* `Program name` is predefined by the institution, the `g3t ping` command will show the available programs
* `Project name` must be unique within the server, and must be alphanumeric and contain no spaces
* For more information, see [creating a project](creating-project.md)

### Add file(s):

```
g3t add folder/file.tsv
g3t add folder/file2.tsv
```

* This command will record an entry for each file in the `MANIFEST` directory.
* Optionally you can tag each file with patient, specimen or other identifiers. See `g3t add --help` for more information.
* Note: `g3t add`, as opposed to git add, creates entries for data files rather than just staging files for a g3t commit.  Your data files remain in place and are not checked in. 
* For more information, see [adding data](upload.md)

### Create meta data

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


> Note: You may supply your own FHIR data, the `g3t meta init` command is a convenience method to generate the required files.

### Check that the metadata is valid:

```
g3t meta validate
```

The system will print summary counts and an informative messages if the metadata is invalid.


### Check that the expected files are queued for upload
```
g3t status
```

### Commit files
```
g3t commit -m "Adding files"
```

### Push to ACED IDP
```
g3t push
```

Notes:

* a push will fail if the metadata is not valid or if the files are not committed.
* a push will fail a data file was already uploaded, use the `--overwrite` option to force an upload.

There are several other options available for the `push` command for specialized use cases. See `g3t push --help` for more information.

### View the upload status (pending or complete)
```
g3t status
```

Check that your data was uploaded successfully by viewing on the Exploration tab on [aced-idp.org](https://aced-idp.org)

## 2. Cloning a project

```commandline
g3t clone [OPTIONS] PROJECT_ID 
```

The clone command will download the MANIFEST/ and META/ directories associated with the project into its own directory.
The `PROJECT_ID` is the name of the project you wish to download.

```commandline
g3t pull
```
The pull command will retrieve the actual data files associated with the manifest. 



Refer to the downloads [page](https://aced-idp.github.io/workflows/portal-download/)
