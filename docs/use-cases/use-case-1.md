---
title: Analyze data, generate metadata and publish to the portal
---

> Note: the tools listed here are under development and may be subject to change

- Assuming that the project exists, files have been uploaded and indexed, and metadata has been created and published.
- Query the system, produce file manifest. An example using the windmill ui:

![image](https://github.com/ACED-IDP/gen3_util/assets/47808/3aa0af6d-0112-4d6b-bb60-3749ef6b482f)

## Download the manifest using `gen3-client`:

### Environment setup

```sh
EXPORT PROJECT_ID=test-Alcoholism

# see https://gen3.org/resources/user/gen3-client/#2-configure-a-profile-with-credentials
EXPORT GEN3_CLIENT_PROFILE=<profile>
```

## Download files using manifest

```sh
gen3-client download-multiple --manifest  manifest-alcoholism.json  --profile $GEN3_CLIENT_PROFILE --filename-format original  --download-path /tmp/alcoholism --numparallel 9  --no-prompt
# ...
# 2023/10/02 12:57:36 14 files downloaded.

```

## Process the downloaded files, using [pydicom](https://pydicom.github.io/) to extract file info

```sh
find /tmp/alcoholism   -name '*.dcm' | xargs -n 1  pydicom show > analysis/dicom-info.txt

```

## Add the resulting analysis file to the manifest

```sh
gen3_util files manifest put analysis/dicom-info.txt
```

## Check manifest

```sh
gen3_util files manifest ls
# - object_id: ...
#  file_name: ...
```

## Upload and index the file

```sh
gen3_util files manifest upload --profile $GEN3_CLIENT_PROFILE
```

## Create metadata for the analysis files. In this example, create minimal metadata using information uploaded into indexd.

```sh
gen3_util meta create indexd meta/
```

## Optionally edit or validate the metadata

```sh
gen3_util meta validate meta
```

## Publish the metadata to the portal

```sh
gen3_util meta publish meta/
```

## Check that your file and meta data have been successfully uploaded

![image](https://github.com/ACED-IDP/gen3_util/assets/47808/c705eb32-f636-42e2-992e-e076f1b28cb8)
