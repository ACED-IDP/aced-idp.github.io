---
title: Creating a Project
---

{% include '/note.md' %}

## Create a project locally and remotely


The g3t init command is used to initialize a new repository in a directory. When you run this command, g3t creates a new subdirectory within the existing directory that houses the internal data structure required for version control. Here's a brief explanation of what happens when you use g3t init:

Initialization: Running g3t init initializes a new Git repository in the current directory. It doesn't affect your existing files; instead, it adds:

* a hidden subfolder `.g3t` within your project that houses the internal data structure required for version control.
* a visible subfolder `META` within your project that houses the FHIR metadata files. 

Existing Files: If you run g3t init in a directory that already contains files, g3t will not overwrite them. 

```
g3t init --help
Usage: g3t init [OPTIONS]

  Create project, both locally and on remote.

Options:
  --project_id TEXT  Gen3 program-project G3T_PROJECT_ID

```

Note the program-project is significant.  It is used to determine the location of the remote repository, bucket storage and access control.  Contact support for more on supported program and project names.

While you can work with an initialized repository locally, **an authorized user will need to sign** the project request before you can push to the remote repository. See `g3t utilities access sign`

## Directory structure
```
.
├── .g3t                                  // local directory for commits, etc.
│   ├── config.yaml
│   └── state
├── META                                  // convention for FHIR metadata
│   ├── DocumentReference.ndjson
│   ├── Patient.ndjson
│   ├── ResearchStudy.ndjson
│   └── ResearchSubject.ndjson
└── <your data here>                      // your research files
    └── ...

```

## Next steps

- [Adding users to a project](./add-users.md)
- [Uploading data to a project](./upload.md)
