# setup-odoopack

GitHub Action that installs the [odoopack](https://github.com/wimwenigerkind/odoopack) CLI and adds it to the `PATH`. Optionally configures authentication for a private odoopack registry.

## Usage

```yaml
- uses: wimwenigerkind/setup-odoopack@v1
  with:
    version: latest
- run: odoopack install
```

### Pin a version

```yaml
- uses: wimwenigerkind/setup-odoopack@v1
  with:
    version: 1.2.0
```

### Private registry

Provide a registry API token (created under *Profile → Tokens* in the registry) and the registry URL it belongs to. The action exports `ODOOPACK_AUTH` scoped to that host for all following steps.

```yaml
- uses: wimwenigerkind/setup-odoopack@v1
  with:
    version: latest
    registry-url: https://registry.example.com
    token: ${{ secrets.ODOOPACK_TOKEN }}
- run: odoopack install
```

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `version` | `latest` | Release tag to install (`1.2.0`, `v1.2.0`, or `latest`). |
| `token` | `''` | API token for a private registry. Requires `registry-url`. |
| `registry-url` | `''` | Base URL the token authenticates against, e.g. `https://registry.example.com`. |

## Outputs

| Output | Description |
| --- | --- |
| `version` | The release tag that was installed. |
| `odoopack-path` | Absolute path to the installed `odoopack` binary. |

## Platforms

Linux, macOS and Windows runners (x86_64 and arm64). Binaries are pulled from the odoopack GitHub Releases and verified against `checksums.txt` when present.
