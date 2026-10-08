# odoo-lint action

A GitHub Action for [odoo-lint](https://github.com/bosd/odoo-lint) (`odl`),
a fast linter for Odoo addons with the checks of pylint-odoo and OCA's
manifest, XML and PO checks.

It installs the prebuilt `odoo-linter` wheel from PyPI with uv, so nothing
is compiled, and fails the job when it finds something.

## Annotations on pull requests

The findings show up on the changed lines of a pull request:

```yaml
name: odoo-lint

on: [push, pull_request]

permissions:
  contents: read

jobs:
  odoo-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
        with:
          persist-credentials: false
      - uses: bosd/odoo-lint-action@v1
```

## Code scanning

With `sarif: true` the findings go to GitHub code scanning instead, which
keeps track of them across runs. It is free for public repositories;
private ones need GitHub Advanced Security.

```yaml
jobs:
  odoo-lint:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
      - uses: actions/checkout@v6
        with:
          persist-credentials: false
      - uses: bosd/odoo-lint-action@v1
        with:
          sarif: true
```

## Inputs

| Input | Default | |
|---|---|---|
| `version` | latest | odoo-linter version from PyPI, e.g. `0.1.0a4` |
| `paths` | `.` | Files or directories to lint, separated by spaces |
| `args` | | More arguments for `odl check`, e.g. `--select PO` or `--odoo-version 18.0` |
| `sarif` | `false` | Send the findings to code scanning instead of annotations |

odoo-lint reads its configuration from `[tool.odoo-lint]` in
`pyproject.toml`, or from `odoo-lint.toml`; see the
[configuration docs](https://odoo-lint.readthedocs.io/en/latest/configuration.html).

Pin `version` to keep the results the same until you update it, and pin the
action to a full commit SHA in your own workflows.

## License

MIT
