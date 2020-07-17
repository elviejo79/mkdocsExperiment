title: Overview

# Rust

Rust is a system programming language sponsored by Mozilla which is a safe, concurrent and practical language supporting functional and imperative-procedural paradigms. The syntax of Rust is similar to C and C++ using blocks of code delimited by curly brackets and control flow keywords such as if, else, while, and for.

Rust targets a field where you probably don't want a garbage collector, but of course memory safety is always important. In order to achieve memory safety without garbage collection, they enforce safety at compile time. Memory gets freed when you don’t need it any more, which the compiler knows for certain because of borrowing and ownership rules. That’s the biggest unique part about Rust: the ownership system.

Cargo is Rust’s build system and package manager. This tool handles a lot of tasks for you, such as building your code, downloading the libraries your code depends on, and building those libraries.

As Rust uses editions to differentiate between major version changes, this documentation tries to keep up with the newest edition but most parts belong to all of them.

To learn about Rust and have a deeper look you should consider reading:

-   [The Book](https://doc.rust-lang.org/book/index.html)
-   [Rust by Example](https://doc.rust-lang.org/rust-by-example/index.html).
-   [The Rust Reference](https://doc.rust-lang.org/reference/index.html)

Further reading may be:

-   [The Cargo Book](https://doc.rust-lang.org/cargo/)
-   [The rustc Book](https://doc.rust-lang.org/rustc/index.html)
-   [The Rustdoc Book](https://doc.rust-lang.org/rustdoc/index.html)
-   [Standard Library](https://doc.rust-lang.org/std/)
-   [WebAssembly](https://rustwasm.github.io/book/introduction.html)

And if this all doesn't help contact the [Community](https://users.rust-lang.org/).

Rust has a steal learning curve with rules around types, ownership, and lifetimes. Despite the difficulty, it's an interesting learning experience but it is worth it.

{!docs/abbreviations.txt!}
