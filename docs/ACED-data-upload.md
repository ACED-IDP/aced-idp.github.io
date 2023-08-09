# Project Creation & Data Upload

## Requirements

- [gen3_util](https://github.com/ACED-IDP/gen3_util): Utilities to manage Gen3 schemas, projects and submissions.

## End user steps:

1. Create the project
    ```sh
    gen3_util projects touch aced-<project>
    ```

2. Add policy to project

    ```sh
    gen3_util projects add policies aced-<project>
    ```

3. Add user to project:

    ```sh
    gen3_util projects add user aced-<project> <email>
    ```

4. Give user permissions to the project

    ```sh
    gen3_util access update <xxx> SIGNED
    ```

5. Import the metadata

    ```sh
    gen3_util meta import dir <DATA DIR> --project_id aced-<project>
    ```

6. Upload the metadata to the S3 Bucket

    ```sh
    gen3_util files cp --duplicate_check --project_id aced-<project> manifest/DocumentReference.ndjson bucket://aced-ohsu-production
    ```

## ETL pod/chart steps:

1. List the metadata available on the S3 Bucket
    ```sh
    gen3_util meta ls
    ```

2. Download the desired metadata
    ```sh
    gen3 file download-single <DID>
    _aced-<project>_meta.zip
    ```

3. Move the metadata to the $HOME/studies directory
    ```sh
    cp _aced-<project>_meta.zip studies 
    unzip studies/_aced-<project>_meta.zip
    ```

4. Upload the data and metadata to the Gen3 endpoint
    ```sh
    ./scripts/load_all <project>
    ```
