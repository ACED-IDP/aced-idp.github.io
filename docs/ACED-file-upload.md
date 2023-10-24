# ACED File Upload

## Requirements

- [Python 3](https://www.python.org/downloads/) — Supported version include >=3.9
- [gen3_util](https://github.com/ACED-IDP/gen3_util) — Utilities to manage Gen3 schemas, projects and submissions

### Installing the Requirements

To install Python 3 on a macOS system, run the following:

```sh
brew install python3
```

To install gen3_util, run the following:

```sh
# Optionally create a virtual environment for the installation
python3 -m venv venv && source venv/bin/activate

# Install gen3_util from PyPi
pip install gen3_util
```

## Upload Steps

The upload steps below use the following example values for the name of project, directory containing the files to upload, and (optionally) an e-mail address used to grant project access to another user:

- Project name: **aced-MyProject**
- Data directory: **aced-files**
- E-mail: **collaborator@aced-idp.org** (optional, only needed if granting project access to other users)

Please substitute these values with your own when running the upload steps.

1. Create the project (e.g. aced-MyProject)

    ```sh
    gen3_util projects touch aced-MyProject
    ```

2. Import the project's metadata

    ```sh
    gen3_util meta import dir ./aced-files --project_id aced-MyProject
    ```

3. Upload the metadata to the the bucket

    ```sh
    gen3_util files cp --duplicate_check --project_id aced-MyProject manifest/DocumentReference.ndjson bucket://aced-ohsu-production
    ```

4. (Optional) Add access policy to the project to allow access to authorized users

    ```sh
    gen3_util projects add policies aced-MyProject
    ```

5. (Optional) Add new user to the project via their e-mail address (e.g. collaborator@aced-idp.org)

    ```sh
    gen3_util projects add user aced-MyProject collaborator@aced-idp.org
    # msg: Approve these requests to assign default policies to aced-MyProject
    # commands:
    # - gen3_util access update 34ccd2cf-48e6-4e55-88ce-70d21ba27a98 SIGNED
    # - gen3_util access update 0a19a6f8-947c-428b-a443-ab983f098734 SIGNED
    ```

6. (Optional) Give user permissions to the project

    ```sh
    gen3_util access update 34ccd2cf-48e6-4e55-88ce-70d21ba27a98 SIGNED
    gen3_util access update 0a19a6f8-947c-428b-a443-ab983f098734 SIGNED
    ```

![Starting the file upload](./images/file-upload.png)
