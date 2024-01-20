
# Cloning a Project

The `g3t clone` command is used to clone a project from the remote repository. Here's a brief explanation of what happens when you use g3t clone:

* A subdirectory is created for the project, it is named after the `project_id`.
* The project is initialized locally, including the `.g3t` and `META` directories.
* The current metadata is downloaded from the remote repository.  
* By default, data files are not downloaded by default:
  * Use the `--data_type all` option to specify `all` files will be downloaded.

```shell
g3t clone --help
Usage: g3t clone [OPTIONS]

  Clone meta and files from remote.

Options:
  --project_id TEXT             Gen3 program-project G3T_PROJECT_ID
  --data_type [meta|files|all]  Clone meta and/or files from remote.
                                [default: meta]

```

## Next steps

- [Downloading data from a project](./portal-download.md)
