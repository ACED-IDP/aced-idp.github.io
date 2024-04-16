
# Quickstart Guide

## 1. Upload Data (To an Existing Project on ACED IDP)
This section outlines how to add data to ACED IDP. Many `g3t` commands behave similarily to `git` commands

Initialize a new project
```
g3t init aced-demo
```

Clone an existing project
```
g3t clone aced-demo
```

Add file(s)
```
g3t add folder/file.tsv
g3t add folder/file2.tsv
```

Create meta data for your files. This `g3t` command will take care of creating this for you
```
g3t utilities meta create
```

Check that the expected files are queued for upload
```
g3t status
```

Commit files
```
g3t commit -m "Adding files"
```

Push to ACED IDP
```
g3t push
```

View the upload status (pending or complete)
```
g3t status
```

Check that your data was uploaded successfully by viewing on the Exploration tab on [aced-idp.org](https://aced-idp.org)

## 2. Download Data

Refer to the downloads [page](https://aced-idp.github.io/workflows/portal-download/)
