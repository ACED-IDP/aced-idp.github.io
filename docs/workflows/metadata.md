# Adding FHIR metadata


## Background

Adding files to a project is a two-step process:

1. Adding file metadata entries to the manifest (see [adding files](add-files.md))
2. Creating FHIR-compliant metadata using the manifest

This page will guide you through the second step of how to associate FHIR data in your g3t project. To understand the FHIR data model, see [FHIR for Researchers](../data-model/introduction.md)

## Generating FHIR Data using g3t

In order to submit metadata to the platform, it needs to be converted into FHIR standard. To do so we have to make use of file metadata entries we had created when we did `g3t add` on our data files.

### Creating metadata files using the manifest

Using the file metadata entries created by the `g3t add` command, `g3t meta init` creates FHIR-compliant metadata files in the `META/` directory, where each file corresponds to a [FHIR resource](https://build.fhir.org/resourcelist.html). At a minimum, this directory will create:

| File                     | Contents                   |
|--------------------------|----------------------------|
| ResearchStudy.ndjson     | Description of the project |
| DocumentReference.ndjson | File information           |


Depending on if a `patient` or `specimen` flag was specified, other resources can be added to the metadata files:

* subjects (ResearchSubject, Patient)
* specimens (Specimen)
* assays (Task)
* measurements (Observation)


* This command will create a skeleton metadata file for each file added to the project using the `patient`, `specimen`, `task`, and/or `observation` flags specified by the `g3t add` command.  
* You can edit the metadata to map additional fields.
* The metadata files can be created at any time, but the system will validate them before the changes are committed.

### Example
To add a cram file that's associated with a subject, sample, and particular task

```sh
g3t add myfile.cram  --patient P0 --specimen P0-BoneMarrow --task_id P0-Sequencing
g3t meta init
```

This will produce metadata with the following relationships:

<img src="/images/create-meta-graph.png" width="100%">

When the project is committed, the system will validate new or changed records. You may validate the metadata on demand by:

```sh
$ g3t  meta validate --help
Usage: g3t meta validate [OPTIONS] DIRECTORY

  Validate FHIR data in DIRECTORY.

```

## Rationale for the `META` directory

All FHIR metadata is housed in the `META/` directory. The convention of using a `META` directory for supporting files a common practice in data management. This directory is used to organize and store the metadata files of your project that describe the Study, Subjects, Specimens, Documents, etc. Here's a brief explanation of this convention:

* Separation of concerns: The `META` directory provides a clear separation between your metadata and other project files. This helps maintain a clean and organized project structure.
* Clarity and Readability: By placing metadata files in a dedicated directory, it becomes easier for researchers (including yourself and others) to locate and understand the main codebase. This improves overall project clarity and readability.
* Build Tools Integration: Many build tools and development environments are configured by default to recognize the `META` directory as the main metadata location. This convention simplifies the configuration process and ensures that tools can easily identify and analyze your data files.
* Consistency Across Projects: Adopting a common convention, such as using `META` for metadata files, promotes consistency across different projects. When researchers work on multiple projects, having a consistent structure makes it easier to navigate and understand each study.
* The 'META' directory will contain files FHIR resource name with the extension [.ndjson](https://www.hl7.org/fhir/nd-json.html). e.g. `ResearchStudy.ndjson`  

## Supplying your own FHIR metadata

In some cases, it might be useful to just supplying your own FHIR metadata without using `g3t add` to create any file metadata. In that case, adding metadata would take on the following flow:

1. Initialize your project
2. Copy external FHIR data as `.ndjson` files to your `META/` directory
3. `git add META/`
4. `g3t commit -m "supplying FHIR metadata"`

This process would be useful for individuals that want to use the system to track relations between metadata but might not necessarily want to connect their actual data files to the system.

## Next Steps

* See the  <a href="/workflows/tabular/">tabular metadata section</a> for more information on working with metadata.
* See the  <a href="/workflows/commit-push/">commit and push section</a> for more information on publishing.

