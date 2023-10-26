---
title: Getting Started
---

> Note: the tools listed here are under development and may be subject to change

Use case: As an analyst, in order to share data with collaborators, I need a way to create a project, upload files and associate those files with metadata. The system should be capable of adding files in an incremental manner.

The following guide details the steps a data contributor must take to submit a project to the ACED data commons.

> In a Gen3 data commons, a semantic distinction is made between two types of data: "data files" and "metadata". [more](https://gen3.org/resources/user/dictionary/#understanding-data-representation-in-gen3)

A "data file" could be information like tabulated data values in a spreadsheet or a fastq/bam file containing DNA sequences. The contents of the file are not exposed to the API as queryable properties, so the file must be downloaded to view its content.

"Metadata" are variables that help to organize or convey additional information about corresponding data files so that they can be queried via the Gen3 data commons’ API or viewed in the Gen3 data commons’ data exploration tool. In a Gen3 data dictionary, variable names are termed "properties", and data contributors provide the values for these pre-defined properties in their data submissions.

For the ACED data commons, we have created a data dictionary based on the FHIR data standard. The data dictionary is available [here](https://github.com/bmeg/iceberg-schema-tools)

## Examples

> In a Gen3 Data Commons, programs and projects are two administrative nodes in the graph database that serve as the most upstream nodes. A program must be created first, followed by a project. Any subsequent data submission and data access, along with control of access to data, is done through the project scope.
> [more](https://gen3.org/resources/operator/#6-programs-and-projects)

For the following examples, we will use the `test` program, please use the `gen3_util projects ls` command to verify what programs you have access to.

## Create a project in authorization system

```sh
export PROJECT_ID=test-myproject  # change this value to your program-project

# request a new project
gen3_util projects new

# Note: before any project is created, the request must be approved.
# A user with appropriate authority signs requests for the current project
gen3_util access sign  # optionally, use the `--username nancy@example.com` to limit the approvals to a specific user
```

## Next steps

At this point you should now be ready to dive further into the ACED-IDP ecosystem. We've outlined a few possible workflows in the following [Use Cases](./use-cases/index.md) section. If you are new to the ACED platform we recommend starting with the following use case:

- [Analyzing data, generating metadata and publishing to the portal](./use-cases/use-case-1.md)
