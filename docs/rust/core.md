# Language Core

This very long page will go over the whole language specification explaining each part in a short form. As already said, for more detailed information read the documents already mentioned at the start of the Rust part.

## Local Variables

They can be defined using `let` which will immediately allocate memory but they have to be initialized with a value first to be used. Both can also be done in one step:

```rust
let x = 5;
```

!!! notice "Reserved names"

    Rust has some [keywords](https://doc.rust-lang.org/reference/keywords.html) which are not possible to be used as variables. If you want to use such, use a raw variable like `fn r#match ...` starting with `r#` on definition and every call. This is mainly used while transferring files to newer editions. It is always better to use another name.

### Constants

All hard coded values, which won't change in any circumstance, should be declared as constant.
They are defined using `const`, have to be initialized on definition and can't be changed later.

### Mutability

To ensure memory safety all variables are immutable by default. So you have to specify if you want to change them later. To set a variable as mutable precede it with the `mut` keyword.

```rust
let x = 5;      // immutable variable
let mut x = 5;  // mutable variable
```

### Shadowing

You can declare a new variable with the same name as a previous variable. Meaning the first variable is shadowed by the second so that the second variable’s value is what appears when the variable is used. You do so by redefining the variable again.

This is often used to change the type of the value but reuse the same name which is not possible with mutable variables.

### Scope

The scope is the range within the program for which the variable is valid. It is valid from it's definition till the end of scope.
The scope of the variable is the block in which it is defined.

A new scope can be created by using curly braces also directly within the function.

## Copying

Data on the stack (primary data types) are copied by value, others on the heap are moved, if you assign one variable to another. That means a moved variable can't be used any longer.

```rust
let a = 1;
let b = a;  // copy data on the stack

let s1 = String::from("hello");
let s2 = s1;            // move data to s2, s1 can no longer be used
let s3 = s2.clone();    // clone data on the heap
```

To really copy heap data you need to `clone` them.

The same goes for the copying/moving variables into functions as parameters or by returning them. So they are no longer valid in the parent function after the call is made.

### Ownership

Rust’s central feature is ownership with the following rules:

-   Each value in Rust has a variable that’s called its owner.
-   There can only be one owner at a time.
-   When the owner goes out of scope, the value will be dropped.
-   The owner may borrow the variable to another function.

Like shown above ownership is managed by copying/moving variables between functions.

Alternatively the variable owned in the parent function can be borrowed to sub functions as reference indicated by `&`. The owner stays and can further work with the variable. Keep in mind that the reference also have to be mutable if it's value should be changeable.

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

You can have only one mutable reference to a particular piece of data in a particular scope. If you need more you have to create separate scopes to end validity for one mutable reference before the other is created. The same goes for mixed mutable and immutable references to prevent you from references which change while using them.

References can't be borrowed out of the definition scope.

## References

Referencing is done by `&` before the variable and dereferencing with `*` before the variable containing a reference.

## Primary Data Types

Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it. But if not we have to annotate it.

```rust
let value: u32 = "42".parse().expect("Not a number!");
```

### Integer

| Length | Type  | Min                  | Max                  |
| ------ | ----- | -------------------- | -------------------- |
| 8-bit  | i8    | -128                 | 127                  |
| 8-bit  | u8    | 0                    | 255                  |
| 16-bit | i16   | -32768               | 32767                |
| 16-bit | u16   | 0                    | 65535                |
| 32-bit | i32   | -2147483648          | 2147483647           |
| 32-bit | u32   | 0                    | 4294967295           |
| 64-bit | i64   | -9223372036854775808 | 9223372036854775807  |
| 64-bit | u64   | 0                    | 18446744073709551615 |
| arch   | isize | ?                    | ?                    |
| arch   | usize | 0                    | ?                    |

The `isize` and `usize` types depend on the kind of computer your program is running on: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

Integer values can be written as:

-   Decimal: `98_222`
-   Hex: `0xff`
-   Octal: `0o77`
-   Binary: `0b1111_0000`
-   Byte (u8 only): `b'A'`

Integer types default is `i32` - this type is generally the fastest, even on 64-bit systems.

If you set a variable you can define the type by also adding the type to the numeric value like `5u43` or with an optional underscore for better readability `5_u32`.

### Float

Rust’s floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respectively. The default type is `f64` because on modern CPUs it’s roughly the same speed as `f32` but is capable of more precision.

Like for integers the `_` may be used as visual separator and the type may be appended like `13_f32`.

### Boolean

Boolean type in Rust are defined as `bool` and have two possible values: `true` and `false`.

### Character

Rust’s `char` type is specified with single quotes and represents a Unicode character.

### Tuple

A `tuple` is an ordered list of multiple other types. To access parts of it you can destructure it or directly access sub parts.

```rust
// define
let coordinate: (i32, i32) = (25, 40);
// destructure
let (x, y) = coordinate;
println!("The value of y is: {}", y);
// direct access
println!("The value of y is: {}", coordinate.1);
```

As far as they contain only primary data types they are completely stored on the stack.

### Array

The `array` gives you a list of values, all with the same type. Arrays in Rust have a fixed length at compile time and cannot grow or shrink.

```rust
let a = [1, 2, 3, 4, 5];
let second = a[1]; // access specific element
```

As far as they contain primary data types they are completely stored on the stack.

### Slices

Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.

A string slice is a reference to part of a String which is created from a String with a range that begins at start and continues up to, but not including the end position, both are optional. The type is written as `&str`.

```rust
let s = String::from("hello world");
let hello = &s[0..5];
```

String literals are also slices pointing to a position in the binary.

Slices are also possible on arrays:

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
```

Here the type is `&[i32]`.

### Literals

Numeric literals can be type annotated by adding the type as a suffix. As an example, to specify that the literal `42` should have the type `i32`, write `42i32`.

### Type Aliasing

The main use of aliases is to reduce boilerplate; for example the `IoResult<T>` type is an alias for the `Result<T, IoError>` type. This is done by defining the type like: `type NanoSecond = u64;`

### Type Conversion

Explicit type conversion (casting) can be performed using the `as` keyword:

```rust
let decimal = 65.4321_f32;
let integer = decimal as u8;
let character = integer as char;
```

Conversions between custom types are implemented using the traits `From` and `Into` or for Strings `ToString` and `FromString`. They are used through the `from()` and `parse()` functions like: `String::from(my_str)` or `"10".parse::<i32>().unwrap()`

## Functions

Functions will be declared using `fn` before it's name. If parameters are possible, they need to be defined with their types.

Within the function you may use statements and expressions. While expressions evaluate to an value, statements only perform some actions. Because assignments are statements, something like `x = y = 6` is not allowed. While function calls are expressions, macros are statements and each expression ending with a `;` is also turned into a statement.

Return values are declared with an arrow `->` before the function body:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
// calling it using
let res = add(5, 7);
```

You can return early from a function by using the return keyword and specifying a value, but most functions return the last expression implicitly. That's why no `;` is set after the addition expression to keep it an expression and return it's result.

### Closures

Sometimes you need anonymous functions given as a closure to another function or variable. They can capture values from the defining scope.
The arguments here are given between pipes `|err|`, instead of round brackets used in normal functions, before a code block:

```rust
let add = |a, b| {
    let x = add(5, 7);
    x
});
// calling it using
let res = add(5, 7);
```

Closures are usually short and relevant only within a narrow context they mostly don't need type annotations.
It can also consist of a single expression without the curly braces:

```rust
let add_one_v4 = |x| x + 1 ;
```

Each variable in the definition scope of the closure is directly accessible:

```rust
let x = 4;
let equal_to_x = |z| z == x;
let res = equal_to_x(4); // that's true
```

Using the `move` keyword before the parameters, the closure will take ownership of the used variables.

A struct can be made to hold the closure and it's result value. This allows to call it multiple times but only execute it once.
When taking a closure as an input parameter, the closure's complete type must be annotated using one of a few traits. In order of decreasing restriction, they are:

-   `Fn`: the closure captures by reference (`&T`)
-   `FnMut`: the closure captures by mutable reference (`&mut T`)
-   `FnOnce`: the closure captures by value (`T`)

See the description of structs and traits later in this book. See the [Caching using Struct](solutions/cache.md) as an example of how this is used.

### Diverging functions

Functions which will never return are called diverging functions. They are marked using !, which is an empty type.

```rust
fn foo() -> ! {
    panic!("This call never returns.");
}
```

## Comments

As in most C-like languages you can have line and block comments:

```rust
// this is a line comment which goes to the end of this line
/* this is a block comment ending on the first closing characters */
```

Additionally there are document comments to add text to the API documentation.

```rust
/// Generate library docs for the following item.
//! Generate library docs for the enclosing item.
```

### Documentation Comments

Documentation comments use three slashes, `///`, instead of two and support Markdown notation for formatting the text. They have to be placed just before the item they’re documenting.

The documentation should explain the element. But don't describe the concrete API because this is added automatically by Rust. Useful sections may be:

-   `Examples` - short code parts to explain typical use
-   `Panics` - scenarios in which the function could panic
-   `Errors` - the kinds of errors that might occur within the `Result`
-   `Safety` - for unsafe functions there should be an explanation why the function is unsafe and covering the invariants that the function expects callers to uphold

Running `cargo test` will run the code examples in your documentation as tests this will keep your examples always functional and up to date.

To document the file itself you can use the alternative marker `//!` which is used to document the element this comment is contained in.

Attention: Documentation in `main.rs` will not be exported. Only files which could be loaded as module can be documented, so put the main documentation in `lib.rs`.

## Control Flow

### if

An `if` expression allows you to branch your code depending on conditions. If the condition is met the following block is executed but if not the optional `else` block is executed. The result of the condition has to be a `bool`.

```rust
let number = 3;
if number < 5 {
    println!("a small number");
} else if number < 10 == 0 {
    println!("a little bigger number");
} else {
    println!("big number");
}
```

As shown multiple alternative conditions may be joined. But if you need multiple alternatives consider using `match`.

`if` is an expression so it can be used in a statement to set the returning value of the evaluated block to a variable.
This means the values that have the potential to be results from each arm of the `if` must be the same type.

```rust
let condition = true;
let number = if condition {
    5
} else {
    6
};
```

### loop

The `loop` keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop using `break`. With `continue` the current loop iteration is stopped and the next iteration will follow.

Also the loop can be named to break or continue out of a specific loop:

```rust
fn main() {
    'outer: loop {
        println!("Entered the outer loop");
       'inner: loop {
            println!("Entered the inner loop");
            break 'outer;
        }
        println!("This point will never be reached");
    }
    println!("Exited the outer loop");
}
```

You have to use a label like `'outer:` before the loop statement and also behind the `break` or `continue`,

Also you might return a value to the rest of the code by putting it after the `break`, and it will be returned by the loop expression.

### while

If you want to check a condition to decide if the loop should continue you may use the `while` loop. Before each round of the loop the condition is evaluated and while it is `true` the loop goes on.

```rust
let mut x = 5;
while x < 10000 {
    println!("{}", x);
    x = x * x
}
```

Alternatively you can do the same using `loop` and an `if` check with `break`.

### for

To iterate a defined number of times use `for n in 1..101` to go from 1 to 100. To also include the second value use `for n in 1..=100` which is the same.

A `for` loop is also used to execute some code for each item in a collection:

```rust
for element in a.iter() {
    println!("the value is: {}", element);
}
```

Alternatively the following iterators may be used in collections:

-   `iter()` - borrows each element of the collection through each iteration, leaving the collection untouched and available for reuse after the loop
-   `into_iter()` - consumes the collection and moves each element into the loop, they are no longer available in the collection
-   `iter_mut()` - mutably borrow each element of the collection, allowing for the collection to be modified in place

## Structs

Structs are one possibility to create objects. They are similar to tuples but with named elements, so that it is clear, what each element is. As a result the order of the elements isn't of matter.

```rust
struct User {
    name: String,
    email: String,
    online: bool,
}
let mut user1 = User {
    email: String::from("someone@example.com"),
    name: String::from("someone"),
    online: true,
};
user1.email = String::from("anotheremail@example.com");

// if variable name and struct field is the same the creation can be simplified
fn build_user(email: String, name: String) -> User {
    User {
        email,
        name,
        online: true,
    }
}
```

To create a new instance partly of the old one the `..` before the variable name specifies that the undefined fields should have the same field as the specified object.

```rust
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};
```

Also you can define structs like tuples without naming the data pieces.

```rust
struct Color(i32, i32, i32);
let black = Color(0, 0, 0);
```

### Methods

Methods are functions contained within a struct, which are bound to the struct object data.

An implementation block `impl` holds method functions for a struct. The method always has a reference to the struct object itself as `&self` as first parameter:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    let area = rect1.area();
}
```

The self reference can also be made mutable. By calling such methods Rust will do automatic dereferencing so there is no need to make references in the calling. As with functions additional parameters are possible.

### Associated Functions

This is like static methods in object oriented languages. They don't get a self reference and are called using `::`

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle { width: size, height: size }
    }
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let sq = Rectangle::square(3);
    let area = sq.area();
}
```

You can also put the methods and associated functions in multiple implementation blocks.

## Enum

The `enum` can be used to present different exclusive states which may also contain an associated value.

```rust
enum IpAddrKind { V4, V6 }

enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
let localhost = IpAddr::V4(127, 0, 0, 1);
```

As shown the second example defines an enum with assigned values. Similar to struct an enum may contain methods.

Rust didn't have the `null` value but the `Option` enum will be used. It is so common that you may use its values `Some` and `None` directly without the `Option::` prefix.

## Pattern Matching

Patterns are a special syntax in Rust for matching against the structure of types, both complex and simple.

Within the patterns you may use:

-   value - only this concrete value
-   `1 | 2` - multiple matches are possible with or
-   `1 ... 5` - ranges
-   `(a, b)` - destructruring vector
-   `{ x: a, y: b }` - destructuring structs
-   `{ x, y }` - destructuring structs, using property name as variable names
-   `_` - is used to ignore values
-   `..` - ignoring remaining or previous parts
-   `ref` or `ref mut` - creating a reference
-   `@` - create variable (before) and also test it (after the @)

### Match

The `match` command allows you to compare a value against a series of patterns and then execute code based on which pattern matches.

```rust
match x {
    None => None,
    Some(i) => Some(i + 1),
}
```

All possible values have to be checked. But you can use the `_` placeholder as pattern to match the rest of the possibilities.

If it gets more complex use match guards with an additional `if`:

```rust
let num = Some(4);
match num {
    Some(x) if x < 5 => println!("less than five: {}", x),
    Some(x) => println!("{}", x),
    None => (),
}
```

### If Let

The `if let` syntax lets you write a singular pattern match in an easier form:

```rust
if let Some(3) = some_u8_value {
    println!("three");
}
```

Here the pattern comes before the equal sign and the value behind. As with a normal if you can also use an else part which is executed if the pattern doesn't match. It is used like `if` and can also be combined with it using `else if let` and `else`.

### While let

Similar to the `if let` the `while let` will run a loop while the match succeeds.

## Error Handling

Rust knows two types of errors:

-   unrecoverable errors - will force the code to stop
-   recoverable errors - will be handled within the code

Use the unrecoverable errors sparsely. If a library use a recoverable error the caller always has the option to make it unrecoverable but not the other way around.

### Unrecoverable errors

Then some bad situations occur which should not be there the `panic!` macro will print a failure message, unwind and clean up the stack and quit the program.

The debug build will have included symbols, so that also a backtrace is possible which shows you the files and lines which brought you to the problem.
If the program is called with the `RUST_BACKTRACE=1` environment setting this backtrace will be displayed.

Use such errors in prototyping to be later replaced or in tests. Or use it than it can't really fail.

### Recoverable errors

Functions which may have an error are returning a `Result` enum which is defined as having two variants, `Ok` with an associated value and `Err` with an associated message.

This can be checked like:

```rust
let f = File::open("hello.txt");

let f = match f {
    Ok(file) => file,
    Err(error) => {
        panic!("There was a problem opening the file: {:?}", error)
    },
};
```

At first this is an recoverable error in the `file::open` method but then it was decided to make it in this situation unrecoverable using the `panic!` macro.

The type of `error` within the `Err` handler here is `io::Error`, which is a struct provided by the standard library. This struct has a method `kind` that holds an `io::ErrorKind` value. The enum `io::ErrorKind` is also from the standard library and has variants representing the different kinds of errors that might result from an IO operation. The relevant variant in this example may be `ErrorKind::NotFound`, which indicates the file doesn’t exist yet. It can be checked like:

```rust
if error.kind() == ErrorKind::NotFound { ... }
```

The `Result<T, E>` has some useful shortcut helpers:

-   `unwrap` - if `OK` return the value and if `Err` then call `panic!`
-   `unwrap_or_else` - like `unwrap` but on `Err` call the code in the given closure
-   `expect` - do the same but use the given error message in panic message

### Propagation

This will let the error bubble up through it's call stack. The following example will check for errors and immediately propagate them:

```rust
let mut f = match f {
    Ok(file) => file,
    Err(e) => return Err(e),
};
```

As a shortcut here the `?` operator may be used:

```rust
let mut f = File::open("hello.txt")?;   // immediately return on Result::Err with it
File::open("hello.txt")?.read_to_string(&mut s)?;   // also possibly in between calls
```

This will also automatically convert the error type to the function's defined returning type. But it can only be used in functions returning a `Result`.

## Generics

Generics are abstract stand-ins to allow different concrete types to be used. Therefore the generics has to be defined in the function signature:

```rust
fn largest<T>(list: &[T]) -> T { ... }
```

This defines that the same type which is given ad reference will be returned. So this can be used with `ì32` or `float`...

Generics can also be used in `struct` or `enum` like:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

On implementations you have to define the generics directly after `imp<T> enum<T>{ ... }`.

The performance of generics is not slower because the compiler will transform the generics to different concrete functions which are used.

### WHERE clauses

A bound can also be expressed using a where clause immediately before the opening `{`, rather than at the type's first mention.

Instead of:

```rust
fn apply<F: FnOnce()>(f: F) {
    f();
}
```

It can be written as:

```rust
fn apply<F>(f: F)
    where F: FnOnce()
{
    f();
}
```

## Traits

Traits are similar to interfaces. They define behavior for multiple types. A `trait` contains the function signatures which has to be met:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

This can be used to ensure that types marked with this trait are only allowed on types with the defined signature. To implement a trait on a type:

```rust
impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

Traits can only be implemented on local types. If a trait should have a default behavior this is defined as functions with body to the trait definition.

### Boundaries

A trait can be used to limit generics to only some types.

```rust
pub fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}
```

Multiple trait bounds can also be set like `T: Summary + Display` but to make this better readable it can also put as `where` condition before the function body:

```rust
fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

You can also use traits to conditional implement methods:

```rust
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) { ... }
}
```

Or directly implement a trait on types matching another trait:

```rust
impl<T: Display> ToString for T { ... }
```

### Phantom Types

A phantom type parameter is one that doesn't show up at runtime, but is checked statically (and only) at compile time.
In combination with generics this can help to ensure type safety.

## Lifetime

References have lifetimes, which are the scope for which this references are valid and help to prevent dangling references.
The default lifetime can be adjusted using annotations.

Problems occur if the reference of an inner variable is borrowed to an outer variable with longer lifetime. To solve this references are annotated with relations to lifetime spaces:

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

Each lifetime for it's own didn't give any help but the relation of different references with the same lifetime defined in the function signature:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str { .. }
```

This defines that all references in the parameters and the return value must have the same lifetime. Because we’ve annotated the returned reference with the same lifetime parameter ``a`, the returned reference will also be valid for the length of the smaller of the lifetimes of`x`and`y`.

Alternatively you can return an owned data type rather than a reference so the calling function is then responsible for cleaning up the value.

Lifetimes are also possible in method definitions or in a `struct` that holds references
But you won't need to add lifetime annotations anywhere because the compiler will add them automatically for known patterns.

### Static

The `'static` lifetime is special, it keeps the references for the whole lifetime of the program. This is used on string literals which are stored in the binary.

## Iterator

You can create an iterator by calling `iter`, `into_iter`, or `iter_mut` on a vector. You can create iterators from the other collection types in the standard library, such as hash map. You can also create iterators that do anything you want by implementing the Iterator trait on your own types.

On iterators you can also use high level operations like:

-   `filter` - to only select elements where the given closure is true
-   `map` - call a closure on each element and replace it with the closure`s result
-   `zip` - combine two iterators together as tuples
-   `collect` - return a vector of values
-   `count` - count the number of elements

And some more.

### Own Iterator

Each iterator has to implement the `next` method:

```rust
impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        self.count += 1;

        if self.count < 6 {
            Some(self.count)
        } else {
            None
        }
    }
}
```

## Object Orientation

Rust is not fully object oriented but you can implement object oriented patterns with it. Structs and enums are containing data and the procedures working on them, so they are objects. Also the implementation can partly be hide by encapsulation with modules and making only some parts public. But inheritance is not really possible. Although polymorphism is possible using the traits.

## Attributes

An attribute is metadata applied to some module, crate or item. This metadata can be used to/for:

-   conditional compilation of code
-   set crate name, version and type (binary or library)
-   disable lints (warnings)
-   enable compiler features (macros, glob imports, etc.)
-   link to a foreign library
-   mark functions as unit tests
-   mark functions that will be part of a benchmark

They are written as `#[attribute]` if they apply for the current module or following item and `#![attribute]` if they apply for the whole crate. Just like with doc comments. They also may get values...

### Compiler Warnings

-   `#[allow(dead_code)]` - to don't warn about unused functions

### Conditional Compiling

This allows to compile parts only on specific settings like OS:

```rust
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
    println!("You are running linux!");
}

#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
    println!("You are *not* running linux!");
}
```

The same may be achieved using the `cfg!` macro.

The condition may be build using nested parts using the following helper:

-   `any` like `#[cfg(any(unix, windows))]`
-   `all` like `#[cfg(all(unix, target_pointer_width = "32"))]`
-   `not` like `#[cfg(not(foo))]`

You can also set another attribute based on a `cfg` variable with `#[cfg_attr(a, b)]`. This will set attribute `b`, but only if attribute `a` is set.

### Custom Conditions

Using Cargo, they get set in the `[features]` section of your Cargo.toml:

```ini
[features]
# no features by default
default = []
# Add feature "foo" here, then you can use it.
# Our "foo" feature depends on nothing else.
foo = []
```

```rust
#[cfg(feature = "foo")]
mod foo {
}
```

Compile this using `cargo build`, no additional flag will be send to the `rustc` compiler (default). But using `cargo build --features "foo"` it will send the `foo` flag to `rustc` and the output will have the `mod foo` in it.

{!docs/abbreviations.txt!}
