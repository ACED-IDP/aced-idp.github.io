---
title: gen3-tracker
---

{% include '/note.md' %}

## Installing g3t

The `gen3-tracker (g3t)` tool requires a working [Python 3](https://www.python.org/downloads/) installation no older than Python 3.12. Run the following in your working directory to install the latest version of g3t from the Python Package Index:

```sh
# Optionally create a virtual environment
python3 -m venv venv; source venv/bin/activate

pip install gen3-tracker
```

You can verify the installation was successful by then running the `g3t` command with the expected output being the [latest version](https://pypi.org/project/gen3-tracker/#history):

```sh
g3t version
```

### Upgrading g3t

This version should match the latest version on the [PyPi page](https://pypi.org/project/gen3-tracker/). If it is out of date, simply run the following to upgrade your local version:

```sh
pip install -U gen3-tracker
```

### Configuration

g3t uses the [gen3-client](https://gen3.org/resources/user/gen3-client/#2-configure-a-profile-with-credentials) configuration flow.

After configuration, you can either specify the `--profile` or set the `G3T_PROFILE=profile-name` environmental variable.

### Testing the configuration

The command `g3t ping` will confirm that the access key and gen3-client have been configured correctly

```sh
g3t ping

msg: 'Configuration OK: Connected using profile:production'
endpoint: https://aced-idp.org
username: someone@example.com
```

### Usage

`g3t  [OPTIONS] COMMAND [ARGS]...`

The following options and environmental variables are synonymous, you may set them as environmental variables or pass them as parameters to the command line.

| option       | environment    | comment             | example |
| ------------ | -------------- | ------------------- | ------- |
| --project_id | G3T_PROJECT_ID | authorization       |         |
| --profile    | G3T_PROFILE    | gen3-client profile |         |
| --format     | G3T_FORMAT     | Output format.      | yaml    |

For example, a `ping` command using the `aced` profile we be `g3t --profile aced ping`

Alternatively, you can set the environmental variables using the `EXPORT` function e.g.:

```sh
export G3T_PROJECT_ID=aced-myproject
```
