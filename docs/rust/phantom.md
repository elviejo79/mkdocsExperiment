# Phantom Types

## DSL type checking

Rust supports generics. A phantom type is a type parameter that isn't used in the definition of the type. Imagine a datatype that is used to represent types in a simple DSL:

```rust
enum T { TI(int), TS(Box<str>) }
```

The type `T` has two constructors. A `TI` representing an integer and a `TS` representing a string. The Box type is used to get an owned pointer to a string allocated on the heap.

We have two functions that operate on this type. One to add two integers and the other to concatenate two strings:

```rust
fn plus (lhs: T, rhs: T) -> T
fn concat (lhs: T, rhs: T) -> T
```

Using these functions looks like:

```rust
let d1 = TI(1);
let d2 = TI(2);
let x = plus(d1, d2);
display(&x);

let d1 = TS(Box::new("Hello, "));
let d2 = TS(Box::new("World"));
let y = concat (d1,d2);
display(&y);
```

Unfortunately with the type definition of `T` it is possible to add an integer and a string:

```rust
let d1 = TI(1);
let d2 = TS(Box::new("Hello, "));
let x = plus(d1, d2);
display(&x);
```

This won't be caught at compile time but, assuming the plus function aborts on incorrect types, will fail at runtime. A full example that demonstrates this available in test1.rs. Compiling with rustc test.rs and running the resulting test produces:

    int: 3
    str: Hello, World
    task <unnamed> failed at 'error', test1.rs:12

We can detect this issue at compile time by using phantom types. This involves changing the `T` type to be:

```rust
enum T<A> { TI(int), TS(Box<str>) }
```

Notice that the type index A does not appear anywhere in the type definition on the right hand side. The definitions of plus and concat are changed to have a different type for the type index A:

```rust
fn plus (lhs: T<int>, rhs: T<int>) -> T<int>
fn concat (lhs: T<~str>, rhs: T<~str>) -> T<~str>
```

We also have to change the code that creates the `TI` and `TS` types. Code like the following will successfully type check:

```rust
let d1 = TI(1);
let d2 = TS(Box::new("hello"));
let x = plus(d1, d2);
```

This is because `d1` and `d2` get their types inferred as `T<int>` due to plus requiring two `T<int>` types. What's needed is to constrain the types of `d1` and `d2` to `T<int>` and `T<Box<str>>` at the point of definition. This can be done with:

```rust
let d1: T<int> = TI(1);
let d2: T<~str> = TS(Box::new("foo"));
let x = plus(d1, d2);
```

Unfortunately this will still work:

```rust
let d1: T<int> = TI(1);
let d2: T<int> = TS(Box::new("foo"));
let x = plus(d1, d2);
```

A better approach is to not use the constructors for the `T<A>` enum and instead create our own:

```rust
fn make_int (i: int) -> T<int> { TI(i) }
fn make_str (s: Box<str>>) -> T<Box<str>> { TS(s) }
```

Using these to create our values will ensure that they have the type of the argument passed to the function. Passing the wrong type is now an error:

```rust
let d1 = make_int(1);
let d2 = make_str(Box::new("foo"));
let x = plus(d1, d2);
```

In real code it's best to ensure that the constructors for `T<A>` aren't public to ensure that callers from outside the module can't create them and subvert our attempts at better type checking.

This code also ensures that the result of a plus cannot be used in a concat and vice versa. The following code produces a compile error:

```rust
fn main() {
  let d1 = TI(1);
  let d2 = TI(2);
  let x = plus(d1, d2);
  display(&x);

  let d1 = TS(Box::new("Hello, "));
  let d2 = TS(Box::new("World"));
  let y = concat (d1,d2);
  display(&y);

  // Compile error here
  let z = concat(x, y);
display(&z);
}
```

The full example is in test2.rs and compiling it results in:

    test2.rs:45:17: 45:18 error: mismatched types:
    expected `T<~str>` but found `T<int>` (expected ~str but found int)
    test2.rs:45 let z = concat(x, y);
    ^

Commenting out the offending code allows the program to compile and run without errors. There is no runtime overhead or additional code generated when using phantom types. The types are erased at compile time and the resulting program is as if they hadn't been used at all - except that you know the code won't have the runtime failure as it passed type checking.

## Safer strings

Another use of phantom types is for providing a taint to string types so they can't be used in an unsafe manner. An example of this is generating HTML template pages as the response to an HTTP request. The page is generated by concatenating various strings together. A string that comes from user input must be properly escaped before it is allowed to be embedded in an HTML page.

In this example we have a type representing an HTML fragment used to compose an HTML page to be generated:

```rust
enum Fragment { Str(Box<str>) }
```

It only has one constructor, Str, which holds the unique string containing the page data. A render_page function displays a complete page:

```rust
fn get_str(s: Fragment) -> Box<str> {
  match s {
    Str(s) => { s }
  }
}

fn render_page(s: Fragment) {
  println(get_str(s));
}
```

Functions exist to generate the head and body of the page with the latter including some form of data obtained elsewhere. Perhaps user input or the output of some other process:

```rust
fn get_head() -> Fragment {
  Str(Box::new("<head></head>"))
}

fn get_body(s: Fragment) -> Fragment {
  Str("<body>" + get_str(s) + "</body>")
}
```

Data from untrusted sources needs to be 'blessed'. This should perform any escaping necessary to make the data safe to embed in the web page. The following is a main program that generates and displays the page:

```rust
fn main() {
  let user_input = get_user_input(~"<script>alert('oops')</script>");
  let page = generate_page(get_head(),
  get_body(user_input));
  render_page(page);
}
```

This compiles and runs. The full example for trying it out is in test3.rs. The output is:

```html
<html><head></head><body><script>alert('oops')</script></body></html>
```

Unfortunately we forgot to call bless and the user input was added unescaped. Ideally this should be a compile time error. If we add a phantom type to the Fragment class we can differentiate between safe and unsafe strings with no runtime overhead:

```rust
struct Safe;
struct Unsafe;

enum Fragment<T> { Str(Box<str>) }
```

All the generation functions now require safe fragments:

```rust
fn generate_page(head: Fragment<Safe>, body: Fragment<Safe>) -> Fragment<Safe>
fn get_head() -> Fragment<Safe>
fn get_body(s: Fragment<Safe>) -> Fragment<Safe>
fn render_page(s: Fragment<Safe>)
fn bless(s: Fragment<Unsafe>) -> Fragment<Safe>
```

The exception is the function that obtains untrusted data. It returns an unsafe fragment:

```rust
fn get_user_input(s: Box<str>) -> Fragment<Unsafe>
```

Now the code that forgets to call bless (in test4.rs) results in a compile error:

    test4.rs:41:36: 41:46 error: mismatched types: expected `Fragment<Safe>`
    but found `Fragment<Unsafe>` (expected struct Safe but found struct Unsafe)
    test4.rs:41 get_body(user_input));
    ^~~~~~~~~~

This approach can be used to help wherever untrusted data shouldn't be mixed with trusted data. Constructing SQL queries and generating URL's are other examples.

## Servo

One of the uses of phantom types in Servo is in the Node type:

```rust
// A phantom type representing the script task's view of this node. Script is able to mutate
/// nodes but may not access layout data.
pub struct ScriptView;

/// A phantom type representing the layout task's view of the node. Layout is not allowed to mutate
/// nodes but may access layout data.
pub struct LayoutView;

/// An HTML node.
///
/// `View` describes extra data associated with this node that this task has access to. For
/// the script task, this is the unit type `()`. For the layout task, this is
/// `layout::aux::LayoutData`.
pub struct Node<View> {
  ...
}
```

A node has fields that should only be accessed or mutated on certain tasks. The script task can mutate nodes but cannot access data related to layout. The layout task is not allowed to mutate the node but can access the layout data. The prevents data races amongst the fields in the node.

The is implemented using phantom types. The node type is indexed over a View which can be ScriptView or `LayoutView`. There are methods implemented for `Node<ScriptView>` which makes them only accessible to the script task:

```rust
impl Node<ScriptView> {
  pub fn ....
}
```

An example of where the layout task gets the `LayoutView` version of a node is in `handle_reflow`. This takes a `Reflow` structure:

```rust
pub struct Reflow {
  /// The document node.
  document_root: AbstractNode<ScriptView>,
  /// The style changes that need to be done.
  damage: DocumentDamage,
  /// The goal of reflow: either to render to the screen or to flush layout info for script.
  goal: ReflowGoal,
  /// The URL of the page.
  url: Url,
  /// The channel through which messages can be sent back to the script task.
  script_chan: ScriptChan,
  /// The current window size.
  window_size: Size2D<uint>,
  /// The channel that we send a notification to.
  script_join_chan: Chan<()>,
}
```

Note the document_root is an `AbstractNode<ScriptView>`. Inside the `handle_reflow` function in the layout task it does:

```rust
/// The high-level routine that performs layout tasks.
fn handle_reflow(&mut self, data: &Reflow) {
  // FIXME: Isolate this transmutation into a "bridge" module.
  let node: &AbstractNode<LayoutView> = unsafe {
    transmute(&data.document_root)
  };
  ...
}
```

The function transmute is a cast. In this case it is converting the `AbstractNode<ScriptView>` to an `AbstractNode<LayoutView>`. The rest of the reflow deals with this so it can be proven to not access the script portions of the node and therefore should not have data races in those parts.
Node simplified

I've attempted to do a simplified example in Rust to test this type of phantom type usage. In layout.rs I have a basic Node class that is indexed over the views:

```rust
pub struct ScriptView;
pub struct LayoutView;

pub struct LayoutData {
  field3: ~str,
}

impl LayoutData {
  pub fn new() -> LayoutData {
    LayoutData { field3: ~"layoutdata" }
  }
}

pub struct Node<View> {
  field1: ~str,
  field2: ~str,

  priv layout_data: Option<@mut LayoutData>
}
```

Note that layout_data is private. A method implemented on `Node<LayoutView>` provides public access to it:

```rust
impl Node<LayoutView> {
  pub fn layout_data(&self) -> Option<@mut LayoutData> {
    self.layout_data
  }
}
```

Only the layout task has the LayoutView of Node so only it can get access to the layout_data method. A LayoutView is obtained by casting (using transmute):

```rust
impl<View> Node<View> {
  pub fn change_view<View>(&self) -> &Node<View> {
    unsafe {
      transmute(self)
    }
  }
}
```

The file `main.rc` contains the main function that tests these layout types and functions:

```rust
mod layout;

pub fn main() {
  use layout::\*;

  let a = ~Node::new();
  std::io::println (a.field1);
  std::io::println (a.field2);
  let b: &Node<LayoutView> = a.change_view();
  match (b.layout_data()) {
    None => { std::io::println("no layout data"); }
    Some (data) => { std::io::println(data.field3); }
  }
}
```

This shows accessing the fields from the ScriptView view of the Node then switching to the LayoutView to get access to the layout_data. Trying to modify the non-layout fields with the LayoutView should fail. Build the example with rustc `main.rc` and make sure `main.rc` and layout.rs are in the current directory.

The Servo code is quite a bit more complex than this but hopefully it helps with understanding the idiom of phantom type usage in nodes. If I've made any mistakes in my understanding of how things work, please let me know!


{!docs/abbreviations.txt!}
