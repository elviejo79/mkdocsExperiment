# Unit Testing

At its simplest, a test in Rust is a function that’s annotated with the `test` attribute.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

Cargo has an test runner to execute them by calling:

    $ cargo test
       Compiling xxxx v0.1.0 (file:///projects/xxxx)
        Finished dev [unoptimized + debuginfo] target(s) in 0.22 secs
         Running target/debug/deps/xxxx-ce99bcc2479f4607

    running 1 test
    test tests::it_works ... ok

    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

       Doc-tests xxxx

    running 0 tests

    test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

As seen above this will run all tests and give a summary line. The second part of the output is for documentation tests , checking that code and documentation is in sync.

## Structure

Rust divides tests into unit tests and integration tests. Unit tests are small and more focused, testing one module in isolation at a time, and can test private interfaces. Integration tests are entirely external to your library and use your code in the same way any other external code would, using only the public interface and potentially exercising multiple modules per test.

### Unit Tests

The convention is to create a module named `tests` in each file to contain the test functions and to annotate the module with `cfg(test)`. This tells Rust to only build this code in test mode.

### Integration Tests

They are used to test the library's external API. They are written in a separate `tests` directory. Each file here will be compiled separately and looks like:

```rust
extern crate adder;

#[test]
fn it_adds_two() {
    assert_eq!(4, adder::add_two(2));
}
```

If submodules are needed they have to be moved into `<name>/mod.rs` to not be interpreted as tests and we include it normally using `mod <name>;`.

To add integration tests to binary crates you have to move most code into `src/lib.rs` which can be tested and have a small wrapper `src/main.rs` which only calls the library.

### Documentation Tests

If you use examples within your API Documentation they will be also tested to ensure they are up to date with your code.

## Value Checking

### Assertions

To check that the code works correctly some assertion macros will help:

- `assert!` - check that the given expression is true
- `assert_eq!` - check that the two given values are equal
- `assert_ne!` - check that the two given values are not equal

Additional to the required one or two parameters all macros will also allow an additional format string and values for a custom failure message. This additional parameters will be given to the `format!` macro for output.

```rust
assert!(
    result.contains("Carol"),
    "Greeting did not contain name, value was `{}`", result
);
```

### Panic

To check if a panic was raised the `#[shoould_panic]` attribute is used. It have to be after the test attribute and can have an additional expect message which have to be a substring of the panic message:

```rust
#[test]
#[should_panic(expected = "Guess value must be less than or equal to 100")]
fn greater_than_100() {
    Guess::new(200);
}
```

## Ignoring

A test can be specified with attribute `#[ignore]` to normally be ignored. This may be useful for expensive tests. They will only run if directly specified or with the `--ignored` flag.

```rust
#[test]
#[ignore]
fn expensive_test() {
    // code that takes an hour to run
}
```


{!docs/abbreviations.txt!}

