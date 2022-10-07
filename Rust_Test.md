# Test

##Continuous Integration

#### Travis CI

To test package on Travis CI, here is a sample `.travis.yml` file:
```yaml
language: rust
rust:
  - stable
  - beta
  - nightly
matrix:
  allow_failures:
    - rust: nightly
```
This will test all three release channels, but any brakage in nightly will not fail overall build.

#### GitHub Actions

To test package on GitHub Actions, here is a sample `.github/workflows/ci.yml` file:
```yaml
name: Cargo Build & Test

on:
  push:
  pull_request:

env:
  CARGO_TERM_COLOR: always

jobs:
  build_and_test:
    name: Rust project - latest
    runs-on: ubuntu-latest
    strategy:
      matrix:
        toolchain:
          - stable
          - beta
          - nightly
    steps:
      - uses: actions/checkout@v3
      - run: rustup update ${{ matrix.toolchain}} && rustup default ${{ matrix.toolchain}}
      - run: cargo build --verbose
      - run: cargo test --verbose
```
This will test all three release channels. Click `"Actions" > "new workflow"` in the GitHub UI and select Rust to add the default configuration to repo.
