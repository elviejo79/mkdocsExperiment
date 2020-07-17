# Caching using Struct

You create a `struct` that will hold the closure and the resulting value of calling the closure. The `struct` will execute the closure only if we need the resulting value, and it will cache the resulting value so the rest of our code doesn’t have to be responsible for saving and reusing the result.

## Simple implementation

```rust
struct Cacher<T>
    where T: Fn(u32) -> u32
{
    calculation: T,
    last: Option<u32>,
    value: Option<u32>,
}
```

Now you add methods to initialize this cache and to retrieve the value:

```rust
impl<T> Cacher<T>
    where T: Fn(u32) -> u32
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            last: 0, // value at start didn't matter
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        // check if argument changed and use only store if last call was the same
        if self.last != Some(v) {
            self.last = Some(v);
            self.value = None;
        }
        match self.value {
            Some(v) && => v,
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            },
        }
    }
}
```

And it should be called like:

```rust
let incrementor = Cacher::new(|a| a + 1);
let x = incrementor.value(5); // x will be 6
```


{!docs/abbreviations.txt!}

