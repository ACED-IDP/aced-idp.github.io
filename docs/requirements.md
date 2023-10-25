---
title: Requirements
---

> Note: the tools listed here are under development and may be subject to change

## Dependencies

- Credentials: `~/.gen3/credentials.json` [identity file](https://gen3.org/resources/user/using-api/#credentials-to-send-api-requests) from the [portal](https://aced-training.compbio.ohsu.edu/identity)

- Download the access key from the portal and save it in the standard location `~/.gen3/credentials.json`
  ![image](https://gen3.org/resources/user/using-api/API_key_profile_page.png)

- A configured [gen3-client](https://gen3.org/resources/user/gen3-client/#2-configure-a-profile-with-credentials) for file uploads and downloads. See the the "gen3-client" section below for download links specific to your operating system.

- A configured gen3_util installation for project management. See the "gen3_util" section for steps on installing this tool.

### gen3-client

| Operating System      | Gen3 Client                          |  Checksum                   |
| --------------------- | ------------------------------------ |  -------------------------- |
| macOS (Apple Silicon) | [gen3-client-macos.zip][macos-arm]   |  [checksums.txt][checksums] |
| macOS (Intel)         | [gen3-client-macos-intel.zip][macos] |  [checksums.txt][checksums] |
| Linux (amd64)         | [gen3-client-linux.zip][linux]       |  [checksums.txt][checksums] |
| Windows (amd64)       | [gen3-client-windows.zip][windows]   |  [checksums.txt][checksums] |

[macos-arm]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos.zip
[macos]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos-intel.zip
[linux]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-linux.zip
[windows]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-windows.zip
[checksums]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/checksums.txt

Download the latest `gen3-client` zip file for your operating system. Unzipping the downloaded file will provide a `gen3-client` executable file that can be run directly.

#### Checksum Verification

In order to verify that the downloaded file can be trusted checksums are provided in [`checksums.txt`][checksums]. See below for examples of how to use this file.

<details>
<summary>Successful Verification</summary>

To verify the integrity of the binaries on macOS run the following command in the same directory as the downloaded file:

```sh
$ shasum -c checksums.txt --ignore-missing
gen3-client-macos.zip: OK
```

If the `shasum` command outputs `OK` than the verification was successful and the executable can be trusted.

</details>

<details>
<summary>Unsuccessful Verification</summary>

Alternatively if the command outputs `FAILED` than the checksum did not match and the binary should not be run.

```sh
$ shasum -c checksums.txt --ignore-missing
gen3-client-macos.zip: FAILED
shasum: WARNING: 1 computed checksum did NOT match
shasum: checksums.txt: no file was verified
```

In such a case please reach out to the contributors for assistance.

</details>


### gen3_util

The `gen3_util` tool requires a working [Python 3](https://www.python.org/downloads/) installation no older than Python 3.8. Run the following in your working directory to install the latest version of gen3_util from the Python Package Index: 

```sh
# Optionally create a virtual environment
$ python3 -m venv venv ; source venv/bin/activate

$ pip install gen3_util
```

You can verify the installation was successful by then running the `gen3_util` command with the expected output being the latest version:

```sh
$ gen3_util
msg: Version 0.0.8
```

#### Testing the configuration

The command `gen3_util ping` will confirm that the access key and gen3-client have been configured correctly

```sh
$ gen3_util ping
msg: Configuration OK, access key found, gen3-client found, gen3-client profile found 'aced'
endpoint: https://aced-idp.org
username: me@myinstitution.edu
```

#### Usage

`gen3_util  [OPTIONS] COMMAND [ARGS]...`

The following options and environmental variables are synonymous, you may set them as environmental variables or pass them as parameters to the command line.

| option                  | environment           | comment             |
| ----------------------- | --------------------- | ------------------- |
| --restricted_project_id | RESTRICTED_PROJECT_ID | authorization       |
| --project_id            | PROJECT_ID            | authorization       |
| --observation_id        | OBSERVATION_ID        | meta data           |
| --patient_id            | PATIENT_ID            | meta data           |
| --specimen_id           | SPECIMEN_ID           | meta data           |
| --task_id               | TASK_ID               | meta data           |
| --profile               | GEN3_PROFILE          | gen3-client profile |

e.g.

    `--observation_id observation-1` or `export OBSERVATION_ID=observation-1`

The following parameters may be used to control system wide behavior:

| environment         | comment                              | default                  |
| ------------------- | ------------------------------------ | ------------------------ |
| GEN3_UTIL_CONFIG    | Path to config file.                 | None                     |
| GEN3_UTIL_FORMAT    | Output format.                       | yaml                     |
| GEN3_UTIL_STATE_DIR | Path for logs and state information. | ~/.gen3/gen3_util        |
| GEN3_API_KEY        | location of credentials.json file.   | ~/.gen3/credentials.json |


## Next steps

With all the dependencies in place you should now be ready to move on to using the `gen3_util` to create and manage your projects. See the [Getting Started](./getting-started.md) page for steps on how to do so.
