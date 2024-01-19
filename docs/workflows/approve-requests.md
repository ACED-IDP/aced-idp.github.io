# Role Based Access Control

# Adding Data Access Committee members

Note: This example uses the ohsu program, but the same process applies to all programs.

* A sysadmin will add the requestor role to a data-access-committee member(s)
* Only users with the requestor role can APPROVE and SIGN adding policies within their program

```text
#  sysadmin adds role to user
g3t access touch <data-access-committee-member>@<my-institution>.edu /programs/<my-institution> --roles requestor_updater_role
```

## Creating a new project

* Any user may request a project be added to the institution's program.
* The user who requested the project is automatically given the read and write roles.
* Ony user's with the requestor_updater_role  can approve and sign a request

## Adding a new user to a project

* Any user may request a user be added to a project.
* The `--write` flag will grant the user write access to the project.
* Ony user's with the requestor_updater_role  can approve and sign a request

```text
g3t utilities access sign --help
Usage: g3t utilities access sign [OPTIONS]

  Sign all policies for a project. 

Options:
  --username TEXT  Sign all requests for user within a project

```


## Use case

There are several institutions that are contributing data to ACED. Each institution has a different set of data access policies. 
Each may have different requirements for how data is accessed, and who can access it. 
Importantly, each institution may have individual who approves access to data.

<img width="894" alt="image" src="https://github.com/ACED-IDP/gen3-helm/assets/47808/77fe3293-f4f4-4aeb-9390-51df7ff042b0">

## Solution

We use Gen3's role based access control (RBAC) to manage access to data.

* There is a separate `program` resource for each institution:
  * /programs/ohsu
  * /programs/stanford
  * /programs/ucl
  * /programs/manchester

Designated users within each institution have privileges to update requests. "Update" in this context means setting the status of a user's request to [SIGNED].

Since this approach relies on Gen3's [Requestor](https://github.com/uc-cdis/requestor/blob/master/docs/functionality_and_flow.md#example-backend-flow) for all assignments of policies to users we get  the following benefits:

* Tooling (command line for now, web page in the future) leverages requestor API
* Auditing of data access requests is done by requestor
