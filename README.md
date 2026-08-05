# github-actions-workflows

Reusable GitHub Actions workflows shared across my projects.

## Workflows

### `rust-ci.yml`

Runs `cargo fmt --check`, `cargo clippy -D warnings`, and `cargo test`
on the stable toolchain.

Inputs (all optional):

- `targets`: extra rustup targets to install (comma-separated)
- `test-env`: environment variables exported before the checks, one
  `KEY=VALUE` per line
- `extra-check`: shell command run after `cargo test`

```yaml
jobs:
  check:
    uses: todesking/github-actions-workflows/.github/workflows/rust-ci.yml@main
    with:
      targets: thumbv7em-none-eabihf
      extra-check: cargo build -p my-no-std-test --target thumbv7em-none-eabihf
```

### `deploy-playground.yml`

Builds a wasm playground and deploys it to GitHub Pages. The caller
must grant `contents: read`, `pages: write`, and `id-token: write`.

Inputs:

- `build-script` (required): command that builds the playground
- `dist-path` (required): directory uploaded as the Pages artifact
- `wasm-pack-version`: wasm-pack version to install (default `0.15.0`)

```yaml
permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    uses: todesking/github-actions-workflows/.github/workflows/deploy-playground.yml@main
    with:
      build-script: ./my-playground/build.sh
      dist-path: my-playground/dist
```

Callers keep their own triggers, permissions, and concurrency settings.
