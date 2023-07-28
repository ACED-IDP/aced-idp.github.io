# Documentation Site for the ACED Data Commons

This mkdocs-based codebase deploys documentation to [aced-idp.github.io](https://aced-idp.github.io).

## ACED Commons Deployments & Sites

| Deployment     | Site                                                                           |
| -------------- | ------------------------------------------------------------------------------ |
| Production     | [aced-idp.org](https://aced-idp.org)                                           |
| Staging        | [staging.aced-idp.org](https://staging.aced-idp.org)                           |
| Development    | [development.aced-idp.org](https://development.aced-idp.org)                   |
| Status Monitor | [aced-idp.github.io/status-monitor](https://aced-idp.github.io/status-monitor) |
| Documentation  | [aced-idp.github.io](https://aced-idp.github.io)                               |

![Main landing page for ACED IDP](./docs/images/main-page.png)


# Local Development

To start the documentation server run the `mkdocs serve` command:

```sh
➜ mkdocs serve

INFO     -  Building documentation...
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.25 seconds
INFO     -  [13:45:40] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO     -  [13:45:40] Serving on http://127.0.0.1:8000/
```

Running on a port other than 8000 is possible with the `--dev-addr <IP:PORT>` flag (e.g. `mkdocs serve --dev-addr 8181` will start the server on localhost:8181).

# Publishing to [aced-idp.github.io](https://aced-idp.github.io)

The documentation site is automatically generated for every push to the main branch. To manually update the site run the `mkdocs gh-deploy --force` command:

```sh
➜ mkdocs gh-deploy --force

INFO     -  Cleaning site directory
INFO     -  Building documentation to directory: /Users/beckmanl/code/aced-idp.github.io/site
INFO     -  Documentation built in 0.49 seconds
INFO     -  Copying '/Users/beckmanl/code/aced-idp.github.io/site' to 'gh-pages' branch and pushing to GitHub.
INFO     -  Your documentation should shortly be available at: https://aced-idp.github.io/
```
