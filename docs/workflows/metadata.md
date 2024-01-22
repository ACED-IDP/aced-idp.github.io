# Adding metadata


### `META` directory

The convention of using a `META` directory for supporting files a common practice in data management. This directory is used to organize and store the metadata files of your project that describe the Study, Subjects, Specimens, Documents, etc. . Here's a brief explanation of this convention:

* Separation of concerns: The `META` directory provides a clear separation between your metadata and other project files. This helps maintain a clean and organized project structure.
* Clarity and Readability: By placing metadata files in a dedicated directory, it becomes easier for researchers (including yourself and others) to locate and understand the main codebase. This improves overall project clarity and readability.
* Build Tools Integration: Many build tools and development environments are configured by default to recognize the `META` directory as the main metadata location. This convention simplifies the configuration process and ensures that tools can easily identify and analyze your data files.
* Consistency Across Projects: Adopting a common convention, such as using `META` for metadata files, promotes consistency across different projects. When researchers work on multiple projects, having a consistent structure makes it easier to navigate and understand each study.
* The 'META' directory will contain files FHIR resource name with the extension [.ndjson](https://www.hl7.org/fhir/nd-json.html). e.g. `ResearchStudy.ndjson`  



### Create metadata files

Every file uploaded to the project must have accompanying metadata in the form of FHIR resources.  The metadata is stored in the `META` directory.  

* The minimum required metadata for a study is the `ResearchStudy` resource.
* The minimum required metadata for a file is the `DocumentReference` resource.

Additional resources can be added to the metadata files:

* subjects (ResearchSubject, Patient)
* specimens (Specimen)
* assays (Task, DiagnosticReport)
* measurements (Observation)

As a convenience, the `g3t utilities meta create` command will create a minimal metadata for each file in the project.

* This command will create a skeleton metadata file for each file added to the project using the `_id` parameters specified on the `g3t add` command.  
* You can edit the metadata to map additional fields.
* The metadata files can be created at any time, but the system will validate them before the changes are committed.

e.g. The `g3t utilities meta create` will use the metadata identifiers specified on the `g3t add` command:

```sh
g3t add myfile.cram  --patient P0 --specimen P0-BoneMarrow --task_id P0-Sequencing
g3t utilities meta create 
```
Will produce metadata with these relationships:

<img src="/images/create-meta-graph.png" width="100%">

When the project is committed, the system will validate new or changed records.  You may validate the metadata on demand by:

```sh
$ g3t utilities meta validate --help
Usage: g3t meta validate [OPTIONS] DIRECTORY

  Validate FHIR data in DIRECTORY.

```

* See the  <a href="/workflows/tabular/">tabular metadata section</a> for more information on working with metadata.
* See the  <a href="/workflows/commit-push/">commit and push section</a> for more information on publishing.

