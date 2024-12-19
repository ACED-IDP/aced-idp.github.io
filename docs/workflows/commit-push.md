# Publishing a Project

## Committing Changes

The `g3t commit` command is used to save your changes to the local repository. Here's a brief explanation of what happens when you use g3t commit:

* Files in the META directory are validated and evaluated for consistency.
* A change set of metadata records are created.
* The commit is saved to the local repository.

```console
$ g3t commit --help
Usage: g3t commit [OPTIONS] [METADATA_PATH]

  Record changes to the project.

  METADATA_PATH: directory containing metadata files to be committed. [default: ./META]

Options:
  -m, --message TEXT  Use the given <msg> as the commit message.  [required]
```

## Viewing the Changes to be committed

See the  <a href="/workflows/status/">status section</a> for more information on working with changes.

## Pushing Changes

The `g3t push` command is used to upload your changes to the remote repository.
Here's a brief explanation of what happens when you use g3t push:

1. Each commit is transferred to the remote repository

2. All files are uploaded to the project bucket

3. A `publish` job is started on the remote server to process the changes

4. Job status can be polled with the `g3t status` command.
    * A job status is returned, the more details job's id can be monitored with the `g3t utilities jobs get <JOB ID>` command.

5. Once the job is complete:
    * The changes are available on the portal
    * The changes are reflected in the `g3t status` command
    * The changes are available to other users for download

```console
$ g3t push --help
Usage: g3t push [OPTIONS]

  Submit committed changes to commons.

Options:
  --overwrite  overwrite files records in index  [default: False]
```

## Updating Files

To update a file that has already been pushed, use the `--overwrite` flag:

```console
$ g3t push --overwrite
```

### When to use `--overwrite`

Use this option if the data upload fails with a `No new files to index` error:

```console
$ g3t push
No new files to index.  Use --overwrite to force
```
