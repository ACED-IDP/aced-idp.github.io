---
title: Creating a Project
---

{% include '/note.md' %}

## Create a project in authorization system

```sh
export PROJECT_ID=aced-myproject  # change this value to your program-project

# request a new project
gen3_util projects new

# Note: before any project is created, the request must be approved.
# A user with appropriate authority signs requests for the current project
gen3_util access sign  # optionally, use the `--username nancy@example.com` to limit the approvals to a specific user
```

## Optional: Granting user access to a project

Once a project has been created you will have full access to it. There are two ways to add additional users to the project:

### 1. Read and Write Access

To give another user full access to the project, run the following:

```sh
gen3_util users add --username someone@example.com --write --project_id aced-myproject
```

### 2.  Read Only Access

Alternatively, to give another user read access only (without the ability to upload to the project), run the following:

```sh
gen3_util users add --username someone@example.com --project_id aced-myproject
```

## Next steps

At this point you should now be ready to dive further into the ACED-IDP ecosystem with the following guides:

- [Uploading data to a project](./upload.md)
- [Downloading data from a project](./download.md)
