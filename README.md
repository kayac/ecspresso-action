# ecspresso-action

GitHub Action to install [ecspresso](https://github.com/kayac/ecspresso), a deployment tool for Amazon ECS.

## Usage

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: kayac/ecspresso-action@v3
        with:
          version: v2.8.8 # or latest
          # version-file: .ecspresso-version
      - run: |
          ecspresso deploy --config ecspresso.yml
```

The action downloads the ecspresso release archive for the runner's architecture, verifies it against `checksums.txt` of the release, and adds the binary to `PATH`.

Only Linux runners (x86_64 and arm64) are supported.

## Inputs

| Name | Description | Default |
|---|---|---|
| `version` | ecspresso version to install (e.g. `v2.8.8`), or `latest` | `latest` |
| `version-file` | File containing the ecspresso version (e.g. `.ecspresso-version`). Takes precedence over `version` | |
| `github-token` | Token used to call the GitHub API when resolving `latest` | `${{ github.token }}` |
| `args` | Arguments to pass to `ecspresso` after installation | |

### `version`

`latest` installs the latest stable ecspresso release (v2.0 or later). The action's own version is independent of the ecspresso version, so `latest` may install a new major version of ecspresso without an action update.

Note: `version: latest` is not recommended as it may cause unexpected behavior when a new version of ecspresso is released. Pin a specific version instead.

### `version-file`

The file contains a version number such as `2.8.8`. A `v` prefix (`v2.8.8`) is also accepted.

### `args`

When `args` is provided, the action runs `ecspresso` with the arguments after installation.

```yaml
      - uses: kayac/ecspresso-action@v3
        with:
          version: v2.8.8
          args: deploy
        env:
          ECSPRESSO_CONFIG: ecspresso.yml
```

`args` is split on whitespace. Quotes are not interpreted, and glob patterns are not expanded. For arguments that need shell quoting, run `ecspresso` in a separate `run:` step instead.

## Migration from `kayac/ecspresso@v2`

The action used to live in the [kayac/ecspresso](https://github.com/kayac/ecspresso) repository. Replace the `uses:` line:

```diff
-      - uses: kayac/ecspresso@v2
+      - uses: kayac/ecspresso-action@v3
```

The inputs are the same. Differences from `kayac/ecspresso@v2`:

- The downloaded archive is verified against `checksums.txt` of the release.
- `args` is no longer evaluated by the shell. See [`args`](#args).
- `version-file` accepts a version with a `v` prefix.
- An invalid `version` (not `latest` or `vX.Y.Z`) is rejected.

`kayac/ecspresso@v3` is kept as a thin wrapper that calls this action and prints a deprecation warning.

## Versioning

Releases are tagged with semver (`v3.0.0`, `v3.1.0`, ...). The major tag (`v3`) points to the latest release of that major version. Full version tags are never moved.

## License

MIT
