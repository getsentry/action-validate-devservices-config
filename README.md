# action-validate-devservices-config

Sets up devservices and validates the configuration for a given service.

Example:

```yml
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
