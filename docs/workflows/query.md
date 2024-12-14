
# Query

## Overview ⚙️

Gen3 supports API access to Files and Metadata, allowing users to download and query their data via the Gen3 SDK and GraphQL queries.

## Quick Start ⚡️

*Adapted from the [Gen3 SDK Quick Start page](https://github.com/uc-cdis/gen3sdk-python/blob/master/docs/tutorial/quickStart.md)*

## 1. Install

The Gen3 SDK is available for installation via PyPi:

```sh
pip install gen3
```

## 2. Authenticate

??? "Configure a Profile with Credentials"

    Download an API Key from the [Profile page](https://aced-idp.org/identity) and save it to `~/.gen3/credentials.json`

    ![Gen3 Profile page](/images/api-key.png)

    ![Gen3 Credentials](/images/credentials.png)

## 3. Query

### Query (`example.graphql`)
```js
query ExampleQuery {
  files: file(first: 1000) {
    file_name
    project_id
    id
  }
  patients: patient(first: 1000) {
    name
    project_id
    id
  }
  observations: observation(first: 1000) {
    code
    project_id
    id
  }
}
```

### Script (`example.py`)
```sh
from gen3.auth import Gen3Auth
from gen3.query import Gen3Query
import json

auth = Gen3Auth()

query = ''

# Read in Example Query
with open('example.graphql') as f:
    query = f.read()

response = Gen3Query.graphql_query(Gen3Query(auth), query_string=query)

formatted = json.dumps(response, indent=2)

print(formatted)

# >>> Example Output
```

### Output

```sh
$ python example.py
{
  "data": {
    "files": [
      {
        "file_name": "example.bam",
        "project_id": "cbds-example",
      },...
    ],
    "patients": [
      {
        "name": "Example Name",
        "project_id": "cbds-example",
      },...
    ],
    "observations": [
      {
        "code": "Example Code",
        "project_id": "cbds-example",
      },...
    ]
  }
}
```

## Additional Resources 📚

- [Gen3 SDK Documentation](https://uc-cdis.github.io/gen3sdk-python/_build/html/index.html)

- [Gen3 SDK Repo](https://github.com/uc-cdis/gen3sdk-python)

- [Gen3 SDK PyPi](https://pypi.org/project/gen3)

- [Guppy Syntax Docs](https://github.com/uc-cdis/guppy/blob/master/doc/queries.md)
