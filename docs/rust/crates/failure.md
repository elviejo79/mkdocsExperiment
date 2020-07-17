# Error Management

Best way to work with errors is to propagate them within the library and let the application code handle it. This is easy to code but in the core misses some information like you will get an 'Permission denied' but don't know on which file.

The following examples will use the `failure` crate to make it easier to work with errors. At the moment this is not really final and development is stagnating but it looks like a new team will come up soon and this will become more popular.

To use it you have to add `failure = "0.1.1"` to your `Cargo.toml` and include it in your `main.rs` and `lib.rs`:

```rust
extern crate failure;
```

## Context

As described before, often the libraries you are using will present error messages that don't provide very helpful information about what exactly has gone wrong. You need to know the circumstances under which it evolves.

If at any processing a `Result` is returned you can add context to it before checking with the `?` operator:

```rust
use failure::{Error, ResultExt};

pub fn run() -> Result<(), Error> {
    let address = "127.0.0.1:80";
    server::new(|| {
        App::new()
            .resource("/", |r| r.f(greet))
            .resource("/{name}", |r| r.f(greet))
    }).bind(address).context(format!("Could not bind {}", address))?
        .start();
    Ok(())
}
```

As seen after the `bind` method you can inject additional context to be carried with this error value, providing semantic information about the nature of the error appropriate to the level of abstraction that the code you are writing operates at. The `context` method on `Fail` takes any displayable value (such as a string) to act as context for this error.

Using the `ResultExt` trait, you can also get `context` as a convenient method on `Result` directly.

In the example above you will see that the "Permission denied" message is not coming from a file but from binding to port 80 as not root user.

## Display

This failures which may be a context information with underlying real error can be best displayed using a short formatter method:

```rust
fn main() {
    if let Err(ref e) = run() {
        eprintln!("Error: {}", pretty_error(e));
        std::process::exit(1);
    }
}

fn pretty_error(err: &failure::Error) -> String {
    let mut pretty = err.to_string();
    let mut prev = err.as_fail();
    while let Some(next) = prev.cause() {
        pretty.push_str(": ");
        pretty.push_str(&next.to_string());
        prev = next;
    }
    pretty
}

// Error: Could not bind 127.0.0.1:80: Permission denied (os error 13)
```

In the main method the occurring error is processed, printed and the program will stop.

The `pretty_error` function will generate a concatenated `String` using the `Error` with all its causes. In the example above this will be the context and the error itself.

## Backtrace

To also support backtrace on errors using the environment setting `RUST_BACKTRACE=1`:

```rust
if let Some(bt) = e.as_fail().backtrace() {
    eprintln!("{}", bt)
}
```

This will only generate and output the backtrace on demand.


{!docs/abbreviations.txt!}

