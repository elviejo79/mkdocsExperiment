# Standard Library

## Formatted Output

Some macros help easily write output:

-   `format!` - write formatted text to String
-   `println!` - write line to standard output
-   `print!` - write formatted text to standard output without additional newline
-   `eprintln!` - write line to error output
-   `eprint!` - write formatted text to error output without additional newline

They all allow to use placeholder:

-   `{}` - for text output using `Display` trait
-   `{:?}` - for debugging using `Debug` trait

Within the curly braces a number or identifier can be added as prefix to specify the element number or variable name.

And for the `Debug` trait a format specifier can be added after the colon separator:

-   fill character with alignment
    -   `<` - left-aligned
    -   `^` - center-aligned
    -   `>` - right-aligned
-   sign
    -   `+` - always print the sign
    -   `#?` - use pretty print as alternate format
    -   `#x` - precede with '0x'
    -   `#X` - precede with '0X'
    -   `#b` - precede with '0b'
    -   `#o` - precede with '0o'
    -   `0` - pad with 0 behind the sign
-   width in characters (can also be given as parameter reference like '0$' or '<name>$')
-   precision count gives the truncate length
    -   `.N` - precision as integer
    -   `.N$` - parameter reference
    -   `.*` - precision defined by next parameter
-   type
    -   `?` - default format
    -   `x` - lower-case hexadecimal
    -   `X` - upper-case hexadecimal

If curly braces should be used as normal character you have to double them as `{{` or `}}`.

## Vector

Vectors allow you to store more than one value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type.

```rust
let v: Vec<i32> = Vec::new();
```

The `vec!` macro is another alternative to create a new vector that holds the values you give it.

```rust
let v = vec![1, 2, 3];
```

To read data from a vector you have two possibilities:

```rust
let v = vec![1, 2, 3, 4, 5];
let third = &v[2];      // &i32
let third = v.get(2);   // Option<&i32>
```

Line 2 gives you an reference to the value while line 3 gives you an `Option` enum with the value. This last access is more secure because you will get the `None` value if the index is out of the current bounds of the vector.

### Update

To update the values of a mutable vector you can use the `push` method:

```rust
let v: Vec<i32> = Vec::new();
v.push(5);
```

### Iterating

A vector is iterateable so you can just use the for loop to step over each element:

```rust
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}
```

### Store different Types

With the use of enum you can trick vectors to store different types as enum can hold different types for you:

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

## String

This is a special collection of UTF-8 characters stored on the heap as growable and mutable data object. The string slice `str` in the core is a fixed string which cannot be changed.

A String is creating through the `new` method or with some initial data using the `to_string` or `String::from` methods:

```rust
let mut s = String::new();
let s = "initial data".to_string();
let s = String::from("initial contents");
```

### Manipulation

Using the `push_str` method you can concat another string to the first one:

```rust
let mut s = String::from("foo");
s.push_str("bar");
```

The `push` methods works identical, but only adds a single character to the string.

The `+` operator can easily be used to concat two strings.

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // Note s1 has been moved here and can no longer be used
```

An alternative is to use the `format!` macro:

```rust
let s = format!("{}-{}-{}", s1, s2, s3);
```

This won't take ownership of any of the variables.

### Index

Access using index numbers is critical because the UTF-8 strings may have characters with more than 1 byte. Better solution is to work with the `chars` method:

```rust
for c in "नमस्ते".chars() {
    println!("{}", c);
}
```

## Hash Map

The `HashMap` stores a mapping of keys and values. But as this is not in the prelude you have to include it first.

You can create an empty hash map with `new` and add elements with `insert` and like in vectors all keys have to have the same type and all values have to have the same type:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

A hash map can be also created from an vector using the `collect` method.

In the example the `insert` is used to add new entries but it can also be used to update an already existing value.

### Access

Similar to vectors you can use the `get` method to retrieve a specific value as `Option`:

```rust
let score = scores.get(&team_name);
```

To insert a value if it doesn't exist this is possible using `entry`:

```rest
scores.entry(String::from("Yellow")).or_insert(50);
```

And as `entry` and `or_insert` returns a mutable reference you can work with it:

```rest
let count = scores.entry(String::from("Yellow")).or_insert(0);
*count += 1;
```

## Smart Pointer

### Box

To hold needed memory on heap low you can use `Box<T>` in any structure to store a pointer to the data structure instead of the whole data structure. This is needed in recursive data structures or there the size of the containing element can vary much. This also implements the `Deref` trait so you can use normal dereferencing with it.

### Reference Counted

When a single value might have multiple owners, Rust has a type called `Rc<T>`, which is an abbreviation for reference counting. Such a value shouldn’t be cleaned up unless it doesn’t have any pointers to it. It keeps track of the number of references to a value which determines whether or not a value is still in use.

To make another instance of an `Rc<T>` instance the call to `Rc::clone` only increments the reference count, which doesn’t take much time. By dropping the variables, the counter will be decreased and by reaching 0 the value will be dropped.

Reference cycles can lead to memory leaks to prevent this you can use `Weak<T>` instead if a back-link in the structure is set. Also there are `Arc<T>` which is an atomic reference counted which is needed for thread safety.

### Interior mutability

This is a design pattern in Rust that allows you to mutate data even when there are immutable references to that data. To mutate data, the pattern uses unsafe code inside a data structure to bend Rust’s usual rules that govern mutation and borrowing but wrapped in a safe API, and the outer type is still immutable. This is implemented in `RefCell<T>`.

### `Deref` Trait

For own types like the `MyBox` here, you have to implement the `Deref` trait to allow type referencing.

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &T {
        &self.0 // MyBox is a tuple with one element
    }
}
```

To work on mutable references you have to implement the `DerefMut` trait.

### `Drop` Trait

With the `Drop` trait you define what to do when a value is about to go out of scope. This can be implemented on any type to release resources like files or network connections.

Rust doesn’t let you call the `Drop` trait’s `drop` method manually so you have to call the `drop` function if you want to force a value to be dropped before the end of its scope.

## Concurrency / Parallelism

Concurrent programming, where different parts of a program execute independently, and parallel programming, where different parts of a program execute at the same time and take advantage of their multiple processors.

Rust standard library only provides an implementation of 1:1 threading there each programming thread belongs to one OS thread but there are crates which allow also M:N threading.

This is only a short overview, so read more about that in the [standard library](https://doc.rust-lang.org/std/) and have a look at the existing crates dealing with concurrency.

### Spawning

To create a new thread, we call the `thread::spawn` function and pass it a closure containing the code we want to run in the new thread.

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        // do something in thread
    });
    // do something in main
    handle.join().unwrap();
}
```

The `join` method on the returned spawn handle will wait till it has finished. The main and thread parts between will run in parallel.

With `move` before the closure's parameter list the ownership will be transfered to the thread.

### Channels

A channel is used to communicate between threads, contains of a transmitter and a receiver and is created using `mpsc::channel`. The way Rust implements channels means a channel can have multiple sending ends that produce values but only one receiving end that consumes those values.

```rust
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();
    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
}
```

By sending a value it is moved, so the variable is no longer allowed after that.
Multiple transmitters are possible by cloning through `let tx2 = mpsc::Sender::clone(&tx);`.

The receiving end of a channel has two useful methods `recv` to wait for a message and `try_recv` which won't block.
You can also use the receiver as an iterator to work on multiple messages, which will end if the channel is closed:

```rust
for received in rx { ... }
```

### `Mutex`

In Rust also shared memory can be done safely. Therefore `Mutex<T>` (is an abbreviation for mutual exclusion) allows only one thread to access some data at any given time. To access the data in a mutex, a thread must first signal that it wants access by asking to acquire the mutex's lock. Using `lock()` you get a mutable reference back which is automatically unlocked on drop of the variable.

## File management

The prelude is loaded to import some common file management methods like `read_to_string` in the following example:

```rust
use std::fs::File;
use std::io::prelude::*;
...
let mut f = File::open(filename).expect("file not found");
let mut contents = String::new();
f.read_to_string(&mut contents).expect("something went wrong reading the file");
```

## CLI Interaction

### Argument parsing

You can use different crates to access parameters but the low level method is to use `env::args`:

```rust
use std::env;
...
let args: Vec<String> = env::args().collect();
```

### Exit

To stop the program without given debugging information like `panic!` will do you can use `process::exit` with the [code](../concepts/exitcodes.md) given to the caller:

```rust
use std::process;
...
process::exit(1);
```

{!docs/abbreviations.txt!}
