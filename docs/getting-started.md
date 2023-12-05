---
title: Getting Started
---

{% include '/note.md' %}

Use case: As an analyst, in order to share data with collaborators, I need a way to create a project, upload files and associate those files with metadata. The system should be capable of adding files in an incremental manner.

The following guide details the steps a data contributor must take to submit a project to the ACED data commons.

> In a Gen3 data commons, a semantic distinction is made between two types of data: "data files" and "metadata". [more](https://gen3.org/resources/user/dictionary/#understanding-data-representation-in-gen3)

A "data file" could be information like tabulated data values in a spreadsheet or a fastq/bam file containing DNA sequences. The contents of the file are not exposed to the API as queryable properties, so the file must be downloaded to view its content.

"Metadata" are variables that help to organize or convey additional information about corresponding data files so that they can be queried via the Gen3 data commons’ API or viewed in the Gen3 data commons’ data exploration tool. In a Gen3 data dictionary, variable names are termed "properties", and data contributors provide the values for these pre-defined properties in their data submissions.

For the ACED data commons, we have created a data dictionary based on the FHIR data standard. The data dictionary is available [here](https://github.com/bmeg/iceberg-schema-tools)

## Examples

> In a Gen3 Data Commons, programs and projects are two administrative nodes in the graph database that serve as the most upstream nodes. A program must be created first, followed by a project. Any subsequent data submission and data access, along with control of access to data, is done through the project scope.
> [more](https://gen3.org/resources/operator/#6-programs-and-projects)

For the following examples, we will use the `aced` program with a project called `myproject`, please use the `gen3_util projects ls` command to verify what programs you have access to.

## Next steps

- [Install gen3-client](gen3-client.md)
