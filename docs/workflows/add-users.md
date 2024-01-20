# Add users

## Granting user access to a project

Once a project has been created you will have full access to it. There are two ways to request the addition additional users to the project:

## 1. Read and Write Access

To give another user full access to the project, run the following:

```sh
g3t utilities users add --username someone@example.com --write --project_id aced-myproject
```

## 2.  Read Only Access

Alternatively, to give another user read access only (without the ability to upload to the project), run the following:

In order to implement these requests, **an authorized user will need to sign** the request before the user can use the remote repository. See `g3t utilities access sign`

```sh
g3t utilities users add --username someone@example.com --project_id aced-myproject
```

## Next steps

At this point you should now be ready to dive further into the ACED-IDP ecosystem with the following guides:

- [Uploading data to a project](./upload.md)
- [Downloading data from a project](./portal-download.md)
