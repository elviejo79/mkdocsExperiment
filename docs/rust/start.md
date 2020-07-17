# Getting started

Start with the setup and installation of your development system to begin learning or coding in Rust.

## Install Rust

On Linux or macOS you only need the installation script:

```bash
$ curl https://sh.rustup.rs -sSf | sh && source $HOME/.cargo/env
info: downloading installer

Welcome to Rust!

This will download and install the official compiler for the Rust programming
language, and its package manager, Cargo.

It will add the cargo, rustc, rustup and other commands to Cargo's bin
directory, located at:

/home/alex/.cargo/bin

This path will then be added to your PATH environment variable by modifying the
profile file located at:

/home/alex/.profile

You can uninstall at any time with rustup self uninstall and these changes will
be reverted.

Current installation options:

default host triple: x86_64-unknown-linux-gnu
    default toolchain: stable
modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>1

info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
info: latest update on 2018-07-10, rust version 1.27.1 (5f2b325f6 2018-07-07)
info: downloading component 'rustc'
72.1 MiB /  72.1 MiB (100 %)  10.0 MiB/s ETA:   0 s
info: downloading component 'rust-std'
56.2 MiB /  56.2 MiB (100 %)  10.1 MiB/s ETA:   0 s
info: downloading component 'cargo'
info: downloading component 'rust-docs'
info: installing component 'rustc'
info: installing component 'rust-std'
info: installing component 'cargo'
info: installing component 'rust-docs'
info: default toolchain set to 'stable'

stable installed - rustc 1.27.1 (5f2b325f6 2018-07-07)

Rust is installed now. Great!

To get started you need Cargo's bin directory ($HOME/.cargo/bin) in your PATH
environment variable. Next time you log in this will be done automatically.

To configure your current shell run source $HOME/.cargo/env
```

Like shown, this will download and install everything for you. Once the Rust installation is complete, Cargo’s bin directory (~/.cargo/bin – where all tools are installed) will be added in your PATH environment variable, in ~/.profile.

This step only needs to be done one time, updates are done using the tool:

```bash
$ rustup update
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
info: checking for self-updates

stable-x86_64-unknown-linux-gnu unchanged - rustc 1.27.1 (5f2b325f6 2018-07-07)
```

## Additional Tools

Some of the additional tools and other common modules needs some of the following to compile:

```bash
sudo apt install pkg-config libssl-dev cmake
```

After that you can now add the following tools are needed also for IDE support:

```bash
rustup toolchain install nightly

rustup component add rls-preview rust-analysis rust-src clippy rustfmt-preview

cargo +nightly install racer
cargo install rustsym
cargo install cargo-update
cargo install cargo-outdated
cargo install cargo-watch
```

See the description of all this [tools](tools.md).

## Editor

As I currently do for all languages I use [VS Code](../env/vscode.md) also for rust. Like a lot of other editors it comes with some plugins to make it a IDE for Rust.

## Updating code

Every few years a new edition will follow and you may upgrade your code to use the new features that are incompatible with the older edition. In order to help transition Cargo can help:

```bash
cargo fix --edition
```

This will check your code, and automatically fix any issues that it can. If cargo fix can't fix something, it will print the warning that it cannot fix to the console. If you see one of these warnings, you'll have to update your code manually.

If `cargo fix --edition` succeeds without warnings you can now switch to the new edition. To do this add the new edition key/value pair to `cargo.toml`.￼

```ini
[package]
name = "foo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"
```

If there's no edition key, Cargo will default to Rust 2015. But in this case, we've chosen 2018, and so our code is compiling with Rust 2018!

{!docs/abbreviations.txt!}
