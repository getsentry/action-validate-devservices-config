# action-validate-devservices-config

Sets up devservices and validates the configuration for a given service.

The action installs the `devservices` version you pass into a Python virtualenv. It then runs these commands in the job's working directory, and a failure in any of them fails the job:

1. `devservices up`
2. `devservices status`
3. `devservices list-dependencies`
4. `devservices logs`
5. `devservices down`

The action does not check out your repository. Run `actions/checkout` before it.

## Usage

This example reads the pinned `devservices` version from `uv.lock`:

```yml
- uses: actions/checkout@v4

- name: Get devservices version
  id: get-devservices-version
  run: |
    awk -F'"' '
      /name/ { pkg = $2 }
      /version/ { if (pkg == "devservices") print "version="$2 }
    ' uv.lock >> $GITHUB_OUTPUT

- uses: getsentry/action-validate-devservices-config@711ae7221998ddf81211f25f5e3873ecffd22387
  name: Validate devservices config
  with:
    devservices-version: ${{ steps.get-devservices-version.outputs.version }}
```

## Inputs

| Input | Required | Default | Description |
| --- | --- | --- | --- |
| `devservices-version` | Yes | None | Version of the `devservices` package to install. |
| `python-version` | No | `3.11` | Python version for the virtualenv. |

The action has no outputs.
