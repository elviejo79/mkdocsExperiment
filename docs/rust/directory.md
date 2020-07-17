# Directory Structure

We follow the best practice structure here which is also made by `cargo`.

The structure can be a workspace with multiple packages (in sub directories) or a single package.

## `Cargo.toml` / `Cargo.lock`

This contains some metadata and is stored in the root of the workspace and package.
[The Manifest Format](https://doc.rust-lang.org/cargo/reference/manifest.html)

## `src`

The main directory for all sources of the library or binary in the package.

- `main.rs` - the start point of a binary
- `lib.rs` - the start point of a library

Modules with submodules will have sub directories. Here the `mod.rs` contains the parent module and the child modules are there as separate files or directories with their files.

## `tests`

Containing integration tests of each package.

## `targets`

Everything created while building, debugging and testing will be made here. It is also the folder you find your created binaries only in the top level workspace or package.


{!docs/abbreviations.txt!}

