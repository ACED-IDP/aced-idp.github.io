---
title: Restrict access to some files
---

> Note: the tools listed here are under development and may be subject to change

Some use cases need to limit to some files to a subset of project members.

For example, given the example above, let's say we needed to restrict access to some files to only certain users.

First, we will set up a second project, by convention with a `_restricted` suffix.

```sh
g3t init --project_id=test-myproject
g3t init --project_id=test-myproject_restricted
```

Next, lets add a user to the primary project, but **not** the restricted project:

```sh
g3t untilities users add --username someone@example.com --project_id=test-myproject
```

Now, create a manifest using one of the methods above.

Then, use the `--restricted_project_id` option to the `g3t push` command to add a second authorization requirement to those files. e.g.

```sh
g3t push --restricted_project_id test-myproject_restricted
```

In order to download files from that manifest, a user must read privileges to **both** test-myproject and test-myproject_restricted.
