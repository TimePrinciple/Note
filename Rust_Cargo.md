# Cargo Book

## Overview

To start a new package with Cargo, use `cargo new`:
```console
$ cargo new <project_name>
```
- `--bin` makes a binary program.
- `--lib` makes a library.

This also initializes a new `git` repository by default.

#### `Cargo.toml`

- If this file describes a package or a workspace, it's a *manifest*.
- If this file only describes a workspace, and does not include a package, it's a *virtual manifest*.

This file contains all the metadata that Cargo needs to compile the package.

#### `Cargo.lock`

This file contains the exact information about which revision of all of these dependencies used. The purpose of a `Cargo.lock` lockfile is to describe the state of the world at the time of a successful build. Cargo uses the lockfile to provide deterministic builds on different times and different systems, by ensuring that the exact same dependencies and versions are used as when the `Cargo.lock` file was originally generated.

- For a non-end product, such as a rust library that other rust packages will depend on, put `Cargo.lock` in `.gitignore`.
- For an end product, which are executable like command-line tool or an application, or a system library with crate-type of `staticlib`, check `Cargo.lock` into `git`.

#### *crate*

A library or executable program is called a crate.
Crates are compiled using the Rust compiler, `rustc`.

#### `rustc`

`rustc` compiles a `.rs` file and generates an executable without the `.rs` suffix. But this command requires the specification of the file name expilcitly.

Most programs will likely to have dependencies and keeping them up to date would be laborious and error-prone if done by hand (by `rustc`).

## Cargo

Cargo is Rust package manager. A tool that allows Rust packages to declare their varioius dependencies and ensure that a repeatable build can always be got.

Cargo does four things:
- Introduces two metadata files with various bits of package information.
- Fetches and builds package's dependencies.
- Invoke `rustc` or another build tool with the correct parameters to build package.
- Introduces conventions to make working with Rust packages easier (e.g. the `cargo build` command).

`cargo build` generates a binary crate in `./target/debug/` directory. If `--release` is specified, the result is in `./target/release` with optimizations turned on.

`cargo run` compiles and then runs the compiled crate in one step.

## Dependencies

[crates.io](https://crates.io/) is the Rust community's central [*package registry*](#Package-registry) that serves as a location to discover and download packages. `cargo` is configured to use it by default to find packages.

To depend on a lirary hosted on crates.io, add it to `Cargo.toml`.

`cargo update` updates the `Cargo.toml` file if there's any dependencies updated.

##Package Layout

Cargo uses conventions for file placement to make it easy to dive into a new Cargo package:

```text
.
├── Cargo.lock
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── main.rs
│   └── bin/
│       ├── named-executable.rs
│       ├── another-executable.rs
│       └── multi-file-executable/
│           ├── main.rs
│           └── some_module.rs
├── benches/
│   ├── large-input.rs
│   └── multi-file-bench/
│       ├── main.rs
│       └── bench_module.rs
├── examples/
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```

- `Cargo.toml` and `Cargo.lock` are stored in the root of package.
- Source code goes in the `src` directory.
- The default library files is `src/lib.rs`.
- The default executable file is `src/main.rs`.
  - Other executables can be placed in `src/bin/`.
- Benchmarks go in the `benches` directory.
- Examples go in the `examples` directory.
- Integration tests go in the tests directory.

Place a `main.rs` file along with extra modules within a subdirectory of the `src/bin`, `examples`, `benches`, or `tests` directory, if any of these consists of multiple source files.

## Cargo Home

The "Cargo Home" functions as a download and source cache. When building a crate, Cargo stores downloaded build dependencies in the Cargo home. This location can be altered by setting the `CARGO_HOME` environmental variable. The `home` crate provides an API for getting this location if the information inside Rust crate is needed.

The Cargo Home consists of following components:

###### Files:
- `config.toml` Cargo's global configuration file.
- `credentials.toml` Private login credentials from `cargo login` in order to log in to a registry.
- `.crates.toml`, `.crates2.json`: These hidden files contain package information of crates installed via `cargo install`.

###### Directories:
- `bin`: The bin directory contains executables of crates that were installed via `cargo installed` or `rustup`. To be able to make these binaries accessible, add the path of the directory to your `$PATH` environment variable.
- `git`: Git sources are stored here
  - `git/db`: When a crate depends on a git repository, Cargo clones the repo as a bare repo into this directory and updates it if necessary. 
  - 'git/checkouts': If a git source is used, the required commit of the repo is checked out from the bare repo inside `git/db` into this directory. This provides the compiler with the actual files contained in the repo of the commit specified for that dependency. Multiple checkouts of different commits of the same repo are possible.
- `registry`: Packages and metadata of crate registries (such as crates.io) are located here.
  - `registry/index`: The index is a bare git repository which contains the metadata (versions, dependencies, etc) of all available crates of a registry.
  - `registry/cache`: Downloaded dependencies are stored in the cache. The crates are compressed gzip archives named with a `.crate` extension.
  - `registry/src`: If a downloaded `.crate` archive is required by a package, it is unpacked into `registry/src` folder where rustc will find the `.rs` files.

## Cargo Reference

#### Specifying Dependencies

##### Specifying dependencies from crates.io

crate.io is the default location Cargo looks for dependencies. Only the name and a version string are required in this case.
```toml
[dependencies]
time = "0.1.12"
```
This actually specifies a range of version and allows SemVer compatible updats. An update is allowed if the new version number does not modify the left-most non-zero digit in the major, minor, patch grouping. In this case, `cargo update -p time` should update to version `0.1.z` if it is the latest release, but would not update to `0.2.0`. 

##### Specifying dependencies from git repositories

To depend on a library located in a `git` repository, the minimum information you need to specify is the location of the repository with the `git` key:
```toml
[dependencies]
regex = { git = "https://github.com/rust-lang-regex", branch = "next" }
```
Cargo assumes that we intend to use the latest commit on the next branch to build package. `tag`, `rev`, `branch` can be combined.

##### Specifying path dependencies

Cargo supports **path dependencies** which are typically sub-crates that live within one repository.
```toml
[dependencies]
hello_utils = { path = "hello_utils" }
```
But crate.io does not allow packages to be published with `path` dependencies.

##### Platform specific dependencies

Under a `target` section.
```toml
[target.'cfg(windows)'.dependencies]
winhttp = "0.4.0"

[target.'cfg(unix)'.dependencies]
openssl = "1.0.1"

[target.'cfg(target_arch = "x86")'.dependencies]
native-i686 = { path = "native/i686" }

[target.'cfg(target_arch = "x86_64")'.dependencies]
native-x86_64 = { path = "native/x86_64" }
```

##### Overriding repository URL

In case the dependencies is not loaded from crates.io, the patched crates' URL needs to be specified. For example, if the dependency is a git dependency, override it to a local path with:
```toml
[patch."https://github.com/path/to/repo"]
my-library = { path = ".../my-library/path" }
```
#### `[patch]`
The `[patch]` section of `Cargo.toml` can be used to override dependencies with other copies. It's syntax is similar to the `[dependencies]` section. 

Cargo only looks at the patch setting in the `Cargo.toml` manifest at the root the workspace. Patch settings defined in dependencies will be ignored.

## Tests

`cargo test` runs tests in two places: in each of `src` fiels and any tests in `tests/`. Tests in `src` files should be unit tests and documentation tests. Tests in `tests/` should be integration-style tests.


#### Package registry

A registry is a service that contains a collection of downloadable crates that can be installed or used as dependencies for a package. The default registry in the Rust ecosystem is [crates.io](https://crates.io/).
