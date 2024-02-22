---
title: gen3-client
---

{% include '/note.md' %}

## Downloading gen3-client

A binary executable of the latest version of the gen3-client should be downloaded from the following Table or from the [Release page on Github](https://github.com/ACED-IDP/cdis-data-client/releases). Choose the file that matches your operating system (Windows, Linux, or macOS).

| Operating System | Gen3 Client                              | Checksum                   |
|------------------|------------------------------------------|----------------------------|
| macOS            | [gen3-client-macos.pkg][macos]           | [checksums.txt][checksums] |
| Linux (amd64)    | [gen3-client-linux-amd64.zip][linux]     | [checksums.txt][checksums] |
| Windows (amd64)  | [gen3-client-windows-amd64.zip][windows] | [checksums.txt][checksums] |


[macos]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-macos.pkg
[linux]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-linux-amd64.zip
[windows]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/gen3-client-windows-amd64.zip
[checksums]: https://github.com/ACED-IDP/cdis-data-client/releases/latest/download/checksums.txt

### Checksum Verification

In order to verify that the downloaded file can be trusted checksums are provided in [`checksums.txt`][checksums]. See below for examples of how to use this file.

<details>
<summary>Successful Verification</summary>

To verify the integrity of the binaries on macOS run the following command in the same directory as the downloaded file:

```sh
$ shasum -c checksums.txt --ignore-missing
gen3-client-macos.pkg: OK
```

If the `shasum` command outputs `OK` than the verification was successful and the executable can be trusted.

</details>

<details>
<summary>Unsuccessful Verification</summary>

Alternatively if the command outputs `FAILED` than the checksum did not match and the binary should not be run.

```sh
$ shasum -c checksums.txt --ignore-missing
shasum: WARNING: 1 computed checksum did NOT match
shasum: checksums.txt: no file was verified
```

In such a case please reach out to the ACED development team for assistance.

</details>

### Installation Instructions

??? "macOS Installation Instructions"
    1. Download the latest macOS version ([dataclient_osx.pkg][macos]).
    2. Follow the instructions in the installer
    3. Open a terminal window.
    4. Move the executable to the default directory: `mv /Applications/gen3-client ~/.gen3/gen3-client`
    5. Add the directory containing the executable to your Path environment variable by entering this command in the terminal: `echo 'export PATH=$PATH:~/.gen3' >> ~/.bash_profile`
    6. Run `source ~/.bash_profile` or restart your terminal.
    7. Now you can execute the program by opening a terminal window and entering the command `gen3-client`

??? "Linux Installation Instructions"
    1. Download the latest Linux version of the gen3-client.
    2. Unzip the archive.
    3. Add the unzipped executable to a directory, for example: `~/.gen3/gen3-client`
    4. Open a terminal window.
    5. Add the directory containing the executable to your Path environment variable by entering this command in the terminal: `echo 'export PATH=$PATH:~/.gen3' >> ~/.bash_profile`
    6. Run `source ~/.bash_profile` or restart your terminal.
    7. Now you can execute the program by opening a terminal window and entering the command `gen3-client`

??? "Windows Installation Instructions"
    1. Download the Windows  version of the gen3-client.
    2. Unzip the archive.
    3. Add the unzipped executable to a directory, for example: `C:\Program Files\gen3-client\gen3-client.exe`
    4. Open the Start Menu and type "edit environment variables".
    5. Open the option "Edit the system environment variables".
    6. In the "System Properties" window that opens up, on the "Advanced" tab, click on the "Environment Variables" button.
    7. In the box labeled "System Variables", find the "Path" variable and click "Edit".
    8. In the window that pops up, click "New".
    9. Type in the full directory path of the executable file, for example: `C:\Program Files\gen3-client`
    10. Click "Ok" on all the open windows and restart the command prompt if it is already open by entering cmd into the start menu and hitting enter.

## Configure a Profile with Credentials

Before using the gen3-client to upload or download data, the `gen3-client` needs to be configured with API credentials downloaded from the [Profile page](https://aced-idp.org/identity).

![Gen3 Profile page](profile.png)

Download the access key from the portal and save it in the standard location `~/.gen3/credentials.json`

![Gen3 Credentials](credentials.png)

From the command-line, run the gen3-client configure command:

```sh
gen3-client configure --profile=<profile_name> --cred=<credentials.json> --apiendpoint=https://aced-idp.org

# Mac/Linux Example:
gen3-client configure --profile=demo --cred=~/Downloads/credentials.json --apiendpoint=https://aced-idp.org

# Windows Example:
gen3-client configure --profile=demo --cred=C:\Users\demo\Downloads\credentials.json --apiendpoint=https://aced-idp.org
```

To confirm you successfully configured a profile with the correct authorization privileges, you can run the `gen3-client auth` command, which should list your access privileges for each project in the commons you have access to:

```sh
gen3-client auth --profile=aced

# 2023/12/05 15:07:12
# You have access to the following resource(s) at https://aced-idp.org:
# 2023/12/05 15:07:12 /programs/aced/projects/myproject...
```
