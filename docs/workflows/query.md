
# Query

## Overview

Gen3 supports API access to Files and Metadata, allowing users to download and query data.

## Quick Start

*Adapted from the [Gen3 SDK Quick Start page](https://github.com/uc-cdis/gen3sdk-python/blob/master/docs/tutorial/quickStart.md)*

## Install

```sh
pip install gen3
```

## 1. Authenticate

??? "Configure a Profile with Credentials"

    Download an API Key from the [Profile page](https://aced-idp.org/identity) and save it to `~/.gen3/credentials.json`

    ![Gen3 Profile page](/images/api-key.png)

    ![Gen3 Credentials](/images/credentials.png)

## 2. Query

```sh
TODO: Add Queries here
```

## Examples

### Metadata

```py
import sys
import logging
import asyncio

from gen3.auth import Gen3Auth
from gen3.metadata import Gen3Metadata

logging.basicConfig(filename="output.log", level=logging.DEBUG)
logging.getLogger().addHandler(logging.StreamHandler(sys.stdout))

def main():
    auth = Gen3Auth(refresh_file="credentials.json")
    mds = Gen3Metadata(auth_provider=auth)

    if mds.is_healthy():
        print(mds.get_version())

        guid = "95a41871-444c-48ae-8004-63f4ed1f0691"
        metadata = {
            "foo": "bar",
            "fizz": "buzz",
            "nested_details": {
                "key1": "value1"
            }
        }
        mds.create(guid, metadata, overwrite=True)

        guids = mds.query("nested_details.key1=value1")

        print(guids)
        # >>> ['95a41871-444c-48ae-8004-63f4ed1f0691']

if __name__ == "__main__":
    main()
```

## Additional Resources

- [Gen3 SDK Dcumentation](https://uc-cdis.github.io/gen3sdk-python/_build/html/index.html)

- [Gen3 SDK Repo](https://github.com/uc-cdis/gen3sdk-python)

- [Guppy Syntax Docs](https://github.com/uc-cdis/guppy/blob/master/doc/queries.md)
