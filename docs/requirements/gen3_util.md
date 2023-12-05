---
title: gen3_util
---

{% include '/note.md' %}

## Installing gen3_util

The `gen3_util` tool requires a working [Python 3](https://www.python.org/downloads/) installation no older than Python 3.10. Run the following in your working directory to install the latest version of gen3_util from the Python Package Index:

```sh
# Optionally create a virtual environment
python3 -m venv venv; source venv/bin/activate

pip install gen3_util
```

You can verify the installation was successful by then running the `gen3_util` command with the expected output being the latest version:

```sh
gen3_util

# msg: Version 0.0.12 
```

### Upgrading gen3_util

This version should match the latest version on the [PyPi page](https://pypi.org/project/gen3-util/). If it is out of date, simply run the following to upgrade your local version:

```sh
pip install -U gen3_util
```

### Testing the configuration

The command `gen3_util ping` will confirm that the access key and gen3-client have been configured correctly

```sh
gen3_util ping

# msg: Configuration OK, access key found, gen3-client found, gen3-client profile found 'aced'
# endpoint: https://aced-idp.org
# username: someone@example.com
```

### Usage

`gen3_util  [OPTIONS] COMMAND [ARGS]...`

The following options and environmental variables are synonymous, you may set them as environmental variables or pass them as parameters to the command line.

| option                  | environment           | comment             | example                        |
| ----------------------- | --------------------- | ------------------- | ------------------------------ |
| --restricted_project_id | RESTRICTED_PROJECT_ID | authorization       |                                |
| --project_id            | PROJECT_ID            | authorization       |                                |
| --observation_id        | OBSERVATION_ID        | meta data           | --observation_id observation-1 |
| --patient_id            | PATIENT_ID            | meta data           |                                |
| --specimen_id           | SPECIMEN_ID           | meta data           |                                |
| --task_id               | TASK_ID               | meta data           |                                |
| --profile               | GEN3_PROFILE          | gen3-client profile |                                |

Alternatively, you can set the environmental variables using the `EXPORT` function:

```sh
export PROJECT_ID=aced-myproject
```

The following parameters may be used to control system wide behavior:

| environment         | comment                              | default                  |
| ------------------- | ------------------------------------ | ------------------------ |
| GEN3_UTIL_CONFIG    | Path to config file.                 | None                     |
| GEN3_UTIL_FORMAT    | Output format.                       | yaml                     |
| GEN3_UTIL_STATE_DIR | Path for logs and state information. | ~/.gen3/gen3_util        |
| GEN3_API_KEY        | location of credentials.json file.   | ~/.gen3/credentials.json |

## Next steps

- [Create a Project](/workflows/creating-project)
