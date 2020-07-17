# Tooling

## Rustup

This is the version manager of rust which will install the Rust language tools for you. Rust is distributed on three different release channels: stable, beta, and nightly. `rustup` is configured to use the stable channel by default.

### Switch between stable and nightly

To switch between the versions use:

    rustup default nightly
    rustup default stable

Or you can call only a single command with nightly:

    rustup run nightly rustc --version

### Update

Rust is released every six weeks, to be up to date you have to call:

    rustup update

This will download and install the newest updates of rust and its core components.

### Components

Components can be removed or added (also using the nightly toolchain):

    rustup component list
    rustup component remove clippy
    rustup component remove --toolchain nightly clippy
    rustup component add clippy
    rustup component add --toolchain nightly clippy

### Cross Compiling

To use cross compiling the standard libraries for all target platforms. The following examples show how to compile on Ubuntu for Windows:

    rustup target add x86_64-pc-windows-gnu

To list possibilities use `rustup target add list`.

Now you need the corresponding linker:

    sudo apt install mingw-w64

And configure it in `~/.cargo/config` and write:

```ini
[target.x86_64-pc-windows-gnu]
linker = "x86_64-w64-mingw32-gcc"
ar = "x86_64-w64-mingw32-gcc-ar"
```

Now to compile for another target:

    cargo build --release --target=x86_64-pc-windows-gnu --verbose

## Cargo

[Cargo](https://doc.rust-lang.org/cargo/) is the package manager and build tool. You can do something like:

-   `cargo new <name>` to create a new binary project
-   `cargo new <name> --lib` to create a new library project
-   `cargo check` to check if code can be compiled
-   `cargo build` to compile the current project
-   `cargo test` run the contained tests
-   `cargo run` to build and start the current project
-   `cargo doc --open` to show documentation
-   `cargo build --release` to build it for release
-   `cargo install <pack>` to install a package
-   `cargo install-update -a` to update all installed modules

### Installing

You can install binary crates by using:

    cargo +nightly install --force racer
    cargo install --force rustsym
    cargo install --force cargo-outdated

The `--force` parameter allows to reinstall (update) if it is already installed.

### Dependencies

Within `Cargo.toml` you specify which dependencies you need:

    [dependencies]
    console = "^0.6.1"
    actix-web = "~0.7"
    clap = "^2.31.2"

You specify the versions like:

-   direct version number
-   `^` prefix - the new version number does not modify the left-most non-zero digit
-   `~` prefix - the specified number has to met but patch-level changes are allowed
-   `*` allows wildcard at the end
-   `< > =` - to manually specify

To show which dependencies are too old:

    cargo outdated

And to update according the defined versions:

    cargo update

### Profiles

Cargo has two predefined and customizable profiles: the `dev` profile Cargo uses when you run `cargo build` and the release profile Cargo uses when you run `cargo build --release`.

To change them define a section `[profile.dev]` or `[profile.release]` in `Cargo.toml`.

### Test

`cargo test` compiles your code in test mode and runs the resulting test binary. If the binary needs some parameters they have to be given after the test parameters separated by `--`.

By default the tests run in parallel so they should not depend on each other or on any shared state, including a shared environment, such as the current working directory or environment variables. Run it synchronously call it with `cargo test -- --test-threads=1`.

If a test passes no output other than the success message is made. If you want to see any output do this using `cargo test -- --test-threads=1 --nocapture`. Synchronous processing is needed else the different test outputs will be mixed up.

If you give specific name to the test runner only these tests containing this substring in its name will be run: `cargo test my_test`

To run also the tests marked as `ignore` you can run `cargo test -- --ignored`.

### Documentation

Using `cargo doc --open` will create the HTML documentation and open it in the browser.

But you may also generate the full documentation only using `cargo doc --release` which will generate the documentation under `target/doc`. You may serve this with any webserver.

### Release

To get a better optimized build which is smaller in size add the following
to `Cargo.toml`:

    [profile.release]
    lto = true        # default is false
    panic = 'abort'   # default is 'unwind'

This will enable the link time optimization and only includes the parts of the
libraries we really used.

### Publishing

Packages can be published as source code `crate` to [crates.io](https://crates.io). To get an API token you have to log in to [crates.io](https://crates.io) using a GitHub account and retrieve your API key from your Account Settings.

    cargo login ThiS0sHoUld0bE0YouR0SeCReT0TOkEN

This stores the API token locally in `~/.cargo/credentials`.

Before publishing you have to set at least `name`, `description` and `license` in the `[package]` section of `Cargo.toml`.

```ini
[package]
name = "guessing_game"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
description = "A fun game where you guess what number the computer has chosen."
license = "MIT OR Apache-2.0"
```

> One major goal of crates.io is to act as a permanent archive of code so that builds of all projects that depend on crates from crates.io will continue to work.
> This means the version can never be overwritten, and the code cannot be deleted.

To finally publish run: `cargo publish`

Although you can’t remove previous versions of a crate, you can prevent any future projects from adding them as a new dependency. This is useful when a crate version is broken for one reason or another. In such situations, Cargo supports yanking a crate version using `cargo yank --vers 1.0.1`

### Extenstion

#### Watch

Watches over your project's source for changes, and runs Cargo commands when they occur.

```bash
cargo install cargo-watch           # install
cargo watch -x test                 # call test on each change
cargo watch -x 'run -- --some-arg'  # run with some arguments
```

#### Update

A cargo subcommand for checking and applying updates to installed executables.

```bash
cargo install cargo-update          # install
cargo install-update -a             # update all installed
```

#### Outdated

A cargo subcommand for displaying when Rust dependencies are out of date.

```bash
cargo install cargo-outdated        # install
cargo-outdated                      # list outdated
```

## Crates

A crate is a compilation unit in Rust, mostly a binary. But it may also be a library.

Rust packages can be found in [crates.io](https://crates.io/search?q=just) and all packages which are released there have their API documentation under [docs.rs](https://docs.rs).

Some popular and useful packages are:

-   [clap](https://clap.rs/) -
    cli interface and argument parser
-   [iron](http://ironframework.io/doc/iron/) -
    HTTP server

## Clippy

A linter

Run it using:

    cargo clippy

If you use linting through `clippy` you may ignore a warning at one point. To do
this use the following syntax which is only read on the `cargo clippy` call which
is also run using `just link` in the alinex projects:

```rust
#[cfg_attr(feature = "cargo-clippy", allow(needless_pass_by_value))]
fn is_u32(v: String) -> Result<(), String> {
    if v.parse::<u32>().is_ok() {
        return Ok(());
    }
    Err(format!("{} isn't a positive integer number", &*v))
}
```

## Rustfmt

A tool for formatting Rust code according to style guidelines.

Mostly your editor will use it to make formatting, but you can run Rustfmt by just typing `rustfmt <filename>`. This runs rustfmt on the given file and related modules. So to run on a whole module or crate, you just need to run on the root file (usually `mod.rs` or `lib.rs`).

## Racer

An auto completion utility

## RLS

A language server implementation for Rust. It provides things like code completion, goto definitions, rich refactoring, and some other nice features.
It enables a much richer development experience and extends on the capabilities of `racer`.

## Optimize Binary

Strips debug symbols if you don't need backtrace:

```bash
strip target/release/hello
```

## Valgrind

[Valgrind](http://valgrind.org) is a tool that can automatically detect many memory management and threading bugs, and profile your programs in detail.

Install using `apt-get -y install valgrind`.

```bash
$ valgrind --leak-check=yes ../target/debug/fibonacci
==28440== Memcheck, a memory error detector
==28440== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==28440== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==28440== Command: ../target/debug/fibonacci
==28440==
Which fibonacci position to calculate?
40
The 40th fibonacci number is 102334155
==28440==
==28440== HEAP SUMMARY:
==28440==     in use at exit: 0 bytes in 0 blocks
==28440==   total heap usage: 0 allocs, 0 frees, 0 bytes allocated
==28440==
==28440== All heap blocks were freed -- no leaks are possible
==28440==
==28440== For counts of detected and suppressed errors, rerun with: -v
==28440== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

```bash
$ valgrind --tool=callgrind ../target/debug/fibonacci
==28492== Callgrind, a call-graph generating cache profiler
==28492== Copyright (C) 2002-2015, and GNU GPL'd, by Josef Weidendorfer et al.
==28492== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==28492== Command: ../target/debug/fibonacci
==28492==
==28492== For interactive control, run 'callgrind_control -h'.
Which fibonacci position to calculate?
40
The 40th fibonacci number is 102334155
==28492==
==28492== Events    : Ir
==28492== Collected : 563309
==28492==
==28492== I   refs:      563,309
```

{!docs/abbreviations.txt!}
