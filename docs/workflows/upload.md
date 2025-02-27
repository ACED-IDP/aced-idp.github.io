---
title: Adding Files
---

{% include '/note.md' %}

## Overview

Adding files to a project is a two-step process:

1. Adding file metadata entries to the manifest
2. Creating FHIR-compliant metadata using the manifest

Briefly, a manifest is a collection of file metadata entries. Just as a ship's manifest is an inventory of its cargo, the `MANIFEST/` directory is an inventory of your file metadata. We update that manifest using `g3t add`. When you `g3t add` a file, an entry is written to a  `.dvc` files in the `MANIFEST` directory, where the dvc file path mirrors the original file path relative to the root of the project. Then, this populated directory is what gets referenced in `g3t meta init` to create FHIR-complaint metadata used to populate the data platforn,

This page will guide you through the first step, detailing the multiple ways to add file metadata to the manifest.

## Adding a local file to the manifest

To add a single file from your current working directory to the manifest,

```bash
g3t add path/to/file
```

In this command, `g3t` creates a metadata entry for the specified data file, automatically calculating metadata like the file's md5sum, type, date modified, size, and path.

## Adding a remote file to the manifest

Sometimes you might want to generate metadata a remote file. To add a file in an Amazon S3 bucket to the manifest,

```sh
g3t add s3://<Bucket>/<File> \
  --etag {ETag} \
  --modified {system_time} \
  --size {system_size} \
  {static_parameters}
```

Since the file is not localized, we need to manually provide some file information, specifically...
1. **Hash:** a unique fixe-length string generated from the file's contents
   1. `etag` is used here but `md5`, `sha1`, `sha256`, `sha512`, and `crc` are also valid.
2. **Date modified:** Date modified in system time
3. **Size:** File size in bytes

To get the ETag, Size, and Date modified for a remote file mirrored on S3, run the following `mc stat` command using the [MinIO client](https://min.io/docs/minio/linux/reference/minio-mc.html):
```sh
mc stat --json ceph/example-bucket/example.bam
{
 "status": "success",
 "name": "example.bam",
 "lastModified": "2024-02-21T09:20:24-08:00",
 "size": 148299010745,
 "etag": "17a5275404b41f52b042b43eb351f5ba-8840",
 "type": "file",
 "metadata": {
  "Content-Type": "application/gzip"
 }
}
```

Then to add the remote file, run the following:

```sh
g3t add s3://example-bucket/file.bam \
  --etag "17a5275404b41f52b042b43eb351f5ba-8840" \
  --size 148299010745 \
  --modified "2024-02-21T09:20:24-08:00" \
  --no-git-add
```

## Associating files with other entities

In some cases, you might want to associate a file to a particular entity like a subject or sample. This can be done with a flag:

```bash
g3t add path/to/file --patient patient_abc
g3t add path/to/file --specimen specimen_001
```

The flag name corresponds to a type of FHIR resource specifically either a [patient](https://build.fhir.org/patient.html), [specimen](https://build.fhir.org/specimen.html), [task](https://build.fhir.org/task.html), or [observation](https://build.fhir.org/observation.html). The info passed into the command after the flag represents the identifier that will be used to group file data on a patient. This can be combined with any of the above methods as an additional flag.

## Adding multiple files to the manifest

Adding multiple files at once is possible as well, here are some examples

```bash
g3t add "dir/*" # recursively add all files in top-level of dir directory to manifest
g3t add "*.txt" # add all .txt files in current directory to manifest
```

Make sure to surround your wildcard string in quotes, as otherwise it will only add the first matching file.

## Migration of existing project files

If you have an existing project that you want to migrate using g3t, you can do so by following these steps:

* Create a new repository using `g3t init`. See [Creating a project](creating-project.md)
* Either move or copy your existing data files into the new repository.  Alternatively, symbolic links are supported.

## A note on data directories

When creating metadata, all paths referring to data directories are stored **relative to the root of the project**. For instance, when doing `g3t add path/to/file.txt`, the output is stored in `MANIFEST/path/to/file.txt`. Here's a brief explanation of this convention:

* Portability: Relative paths make your project more portable, meaning that it can be moved to different locations or shared with others without causing issues with file references. This is particularly important in data engineering projects where datasets and files may be stored in different locations.
* Ease of Collaboration: When working on a data engineering project with multiple team members, using relative paths ensures that everyone can run the code without having to modify file paths based on their local directory structure. This promotes smoother collaboration.
* Consistency Across Environments: Data engineering projects often involve processing large datasets, and the code needs to run consistently across different environments (e.g., development, testing, production). Relative paths help maintain this consistency by allowing the code to reference files and directories relative to the project's root.


## Next steps

* See the  <a href="/workflows/status/">status command</a> for more information on how to verify the manifest.
* See [metadata workflow](metadata.md) for more information on how to create and upload metadata.
