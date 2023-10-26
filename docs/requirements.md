---
title: Requirements
---

> Note: the tools listed here are under development and may be subject to change

- A Python installation with version 3.10 or later

- A configured `gen3-client` for file uploads and downloads. See the the "[gen3-client](#gen3-client)" section below for download links specific to your operating system.

- A configured `gen3_util` installation for project management. See the "[gen3_util](#gen3_util)" section for steps on installing this tool.

## gen3-client

> This section is adapted from the ['Download and Upload Files Using the Gen3-client'](https://gen3.org/resources/user/gen3-client/) page from the main [Gen3 website](https://gen3.org/). This site has a wealth of information that supplements the ACED-IDP project including in-depth examples and usages of the gen3-client. 

### 1. Installation Instructions

A binary executable of the latest version of the gen3-client should be downloaded from the following Table or from the [Release page on Github](https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos.zip). Choose the file that matches your operating system (Windows, Linux, or macOS).

No installation is necessary. Simply download the correct version for your operating system and unzip the archive. The program is then executed from the command-line by running the command gen3-client <options>. For more detailed instructions, see the section below for your operating system.


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

> Note: Do not try to run the program by double-clicking on it. Instead, execute the program from within the shell / terminal / command prompt. The program does not provide a graphical user interface (GUI) at this time; so, commands are sent by typing them into the terminal.

### Checksum Verification

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

#### MacOS / Linux Installation Instructions

1. Download the latest macOS ([Apple Silicon](https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos.zip) or [Intel](https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos-intel.zip)) or [Linux](https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-linux.zip) version of the gen3-client.
2. Unzip the archive.
3. Add the unzipped executable to a directory, for example: `~/.gen3/gen3-client`
4. Open a terminal window.
5. Add the directory containing the executable to your Path environment variable by entering this command in the terminal: `echo 'export PATH=$PATH:~/.gen3' >> ~/.bash_profile`
6. Run `source ~/.bash_profile` or restart your terminal.
7. Now you can execute the program by opening a terminal window and entering the command `gen3-client`

> Note: If your macOS does not allow access, you need to manually allow it under System Preferences > Security & Privacy > General (click the lock icon to unlock, enter administrator name and password).

#### Windows Installation Instructions

1. Download the [Windows](https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-windows.zip) version of the gen3-client.
2. Unzip the archive.
3. Add the unzipped executable to a directory, for example: `C:\Program Files\gen3-client\gen3-client.exe`
4. Open the Start Menu and type "edit environment variables".
5. Open the option "Edit the system environment variables".
6. In the "System Properties" window that opens up, on the "Advanced" tab, click on the "Environment Variables" button.
7. In the box labeled "System Variables", find the "Path" variable and click "Edit".
8. In the window that pops up, click "New".
9. Type in the full directory path of the executable file, for example: `C:\Program Files\gen3-client`
10. Click "Ok" on all the open windows and restart the command prompt if it is already open by entering cmd into the start menu and hitting enter.

#### View the Help Menu

To check that your copy of the client is working and confirm the version, the tool can be run on the command-line in your terminal or command prompt by entering `gen3-client`. Typing this alone or `gen3-client help` will display the help menu. For help on a particular command, enter: `gen3-client <command> help`. Note that you must provide the full path of the tool in order for the commands to run, for example, `./gen3-client` while working from the directory containing the client. Alternatively, you can add the location of the gen3-client executable to your shell’s PATH environment variable.

### 2. Configure a Profile with Credentials

Before using the gen3-client to upload or download data, the gen3-client needs to be configured with API credentials downloaded from the user’s data commons Profile:

- Credentials: `~/.gen3/credentials.json` [identity file](https://gen3.org/resources/user/using-api/#credentials-to-send-api-requests) from the [portal](https://aced-idp.org/identity)

- Download the access key from the portal and save it in the standard location `~/.gen3/credentials.json`

![Gen3 Credentials](https://gen3.org/resources/user/using-api/API_key_profile_page.png)

2. From the command-line, run the gen3-client configure command with the `--cred`, `--apiendpoint`, and `--profile` flags (see examples below).

Example Usage:

```sh
gen3-client configure --profile=<profile_name> --cred=<credentials.json> --apiendpoint=https://aced-idp.org

# Mac/Linux:
gen3-client configure --profile=demo --cred=~/Downloads/demo-credentials.json --apiendpoint=https://aced-idp.org

# Windows:
gen3-client configure --profile=demo --cred=C:\Users\demo\Downloads\demo-credentials.json --apiendpoint=https://aced-idp.org
```

When successfully executed, this will create a configuration file, which contains all the API keys and URLs associated with each commons profile configured, located in the user folder:

```sh
# Mac/Linux:
/Users/demo/.gen3/gen3_client_config.ini

# Windows: 
C:\Users\demo\.gen3\gen3_client_config.ini
```

> **NOTE**: These keys must be treated like important passwords; never share the contents of the credentials.json and gen3-client gen3_client_config.ini or config file!

To confirm you successfully configured a profile with the correct authorization privileges, you can run the gen3-client auth command, which should list your access privileges for each project in the commons you have access to. For example:

```sh
~> gen3-client auth --profile=demo

2019/11/19 11:59:04
You have access to the following project(s) at https://aced-idp.org:
2019/11/19 11:59:04 CPTAC [read read-storage]
2019/11/19 11:59:04 DCF [create delete read read-storage update upload write-storage]
```

## gen3_util

The `gen3_util` tool requires a working [Python 3](https://www.python.org/downloads/) installation no older than Python 3.10. Run the following in your working directory to install the latest version of gen3_util from the Python Package Index: 

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

### Testing the configuration

The command `gen3_util ping` will confirm that the access key and gen3-client have been configured correctly

```sh
$ gen3_util ping
msg: Configuration OK, access key found, gen3-client found, gen3-client profile found 'aced'
endpoint: https://aced-idp.org
username: me@myinstitution.edu
```

### Usage

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
