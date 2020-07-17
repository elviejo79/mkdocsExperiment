# Modules

To split your code into organized and reusable parts you can extract functions and other code into modules. Definitions within the module can be public or private. By default everything in a module has private visibility.

The module itself is created using `mod` with the name off the module and the contents as code block:

```rust
mod network {
    fn connect() {
    }
}
```

## Visibility

Only the public items of a module can be accessed from outside the module scope. You mark functions as public using the `pub` modifier before:

```rust
mod network {
    pub fn connect() {
    }
}
```

The same goes for structs, their fields and methods.

## Files

Modules can be moved into separate files to have better overview over the code. As an example you place a client module's code into `client.rs` file and include it in the main like this:

```rust
mod client;
```

Modules can also be set within other modules. Modules with submodules will be made as directories and the `mod.rs` contains the parent module and the child modules are there as separate files or directories with their files.

## Namespaces

To use other namespaces you can always specify them completely. But to make it easier you may import the namespace with `use` into the current file. After that you can directly use them without the full name.

```rust
use a::series::of;
```

Within modules you can also use relative paths from

- the current module, or type it with `self::`
- the current crate with `crate::`
- the parent module using `super::` as prefix
- the root using `::` as prefix

Using asterisk you can also import all names of a namespace but this may be problematic.

## Libraries

To make it more reusable also for other projects also make a library using `cargo new <name> --lib`.
This will create a library which can be included in different other binaries or libraries. The main file of the library is `lib.rs` which will look like the following for a start:

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

### Usage

To use a library it have to be added in `Cargo.toml`. It can also be imported using `extern crate xxxx;` within the `main.rs` or `lib.rs` but this mostly is not necessary.
After this the namespace can be imported using `use`.

### Reexports

To make elements deep in your structure easily selectable for the libraries uses then can be reexported by `pub use <fullpath>`. After that they can directly be used under the library.

## Workspace

A workspace is a set of related packages that are developed in tandem. Within the workspace directory there should be a `Cargo.toml` file that will configure the entire workspace. This file will have a `[workspace]` section that will allow us to add members to the workspace by specifying the path to them:

```ini
[workspace]
members = [
    "adder",
    "add_lib",
]
```

Within this directory the packages will be created using `cargo new`.

The workspace has one target directory at the top level for the compiled artifacts of all packages to be placed into. Build and tests from the top level directory will always do it for all. Within tests you may select the package to test with `cargo test -p add-one`.

Within the packages you have to define the dependencies between the workspace packages with the relative path:

```ini
[dependencies]
add_lib = { path = "../add_lib" }
```


{!docs/abbreviations.txt!}

