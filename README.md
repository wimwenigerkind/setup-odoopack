# setup-odoopack

GitHub Action that installs the [odoopack](https://github.com/wimwenigerkind/odoopack) CLI and adds it to the `PATH`. Optionally configures authentication for a private odoopack registry.

## Usage

```yaml
- uses: wimwenigerkind/setup-odoopack@v0
  with:
    version: latest
- run: odoopack install
```

`@v0` is a moving tag that always points at the newest `v0.x.y` release of this action. It is moved by hand after each release, so `@v0` picks up fixes without changing your workflow. Pin a full tag such as `@v0.2.0` (or a commit SHA) if you want the action itself to stay fixed.

### Pin a version

`version` selects the odoopack CLI release, independent of the action's own tag. The value below is an example; pick a tag from the [odoopack releases](https://github.com/wimwenigerkind/odoopack/releases).

```yaml
- uses: wimwenigerkind/setup-odoopack@v0
  with:
    version: 0.1.0
```

### Private registry

Provide a registry API token (created under *Profile → Tokens* in the registry) and the registry URL it belongs to. The action exports `ODOOPACK_AUTH` scoped to that host for all following steps. The registry URL must use https; plain http is accepted for localhost only, because odoopack refuses to send a token anywhere else over http.

```yaml
- uses: wimwenigerkind/setup-odoopack@v0
  with:
    version: latest
    registry-url: https://registry.example.com
    token: ${{ secrets.ODOOPACK_TOKEN }}
- run: odoopack install
```

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `version` | `latest` | odoopack release to install (`0.1.0`, `v0.1.0`, or `latest`). |
| `token` | `''` | API token for a private registry. Requires `registry-url`. |
| `registry-url` | `''` | Base URL the token authenticates against, e.g. `https://registry.example.com`. |

## Outputs

| Output | Description |
| --- | --- |
| `version` | The release tag that was installed. |
| `odoopack-path` | Absolute path to the installed `odoopack` binary. |

## Platforms

Linux, macOS and Windows runners (x86_64 and arm64). Binaries are pulled from the odoopack GitHub Releases and verified against the release's `odoopack_<version>_checksums.txt`; the step fails if that file or the binary's entry in it is missing. The auth step uses `jq`, which the GitHub-hosted Linux, macOS and Windows runners ship with; self-hosted runners need it on the `PATH`.
