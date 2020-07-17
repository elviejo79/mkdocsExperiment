title: clap

# clap - Command Line Argument Parser

This is an efficient and configurable library for parsing command line arguments. It supports advanced features like argument relationships, subcommands and mmuch more.

It will not only parse but also validate values and display the help page.

The following is only a short introduction with some examples. Read more about in the [API Docs](https://docs.rs/clap).

## Configuration

You have different possibilities to setup your application. At first the builder style is the most enhanced system. But you can also load the arguments definition from a YAML file or use the macro to do it from a compressed text form inline.

To be really clear and easy readable I use the builder pattern like:

```rust
#[macro_use]
extern crate clap;

fn main() {
    let args = clap::App::new(crate_name!())
       .version(crate_version!())
       .about(crate_description!())
       .author(crate_authors!())
       .get_matches();
}
```

The macros are enabled for clap to use meta data from the `Cargo.toml` file like:

-   `crate_name!()`
-   `crate_version!()`
-   `crate_description!()`
-   `crate_authors!()`

## Setup Application

Basic setting methods:

-   `name` - Program's name to be displayed in help or version information.
-   `version` - Version number to be displayed in version or help information.
-   `about` - String describing what the program does for the help information.
-   `author` - Author(s) that will be displayed to the user when they request the help information.

Advanced setting methods:

-   `usage` - Overwrite auto generated usage line.
-   `help` - Replaces the entire help message.

## Arguments

Use `arg` method to add an argument like described below to be added to the list of valid possibilities.

Argument definition:

```rust
clap::Arg::with_name("config")
    .short("c")
    .long("config")
    .takes_value(true)
    .value_name("FILE")
    .help("Provides a config file to myprog");
```

Methods for arguments:

-   `short` - Short version of the argument without the preceding `-` as option.
-   `long` - Long version of the argument without the preceding `--` as option.
-   `alias` - Alternative option name, which is "hidden" from help information.
-   `visible_alias` - Alias option name, which is visible in help information.
-   `help` - Short one line help text.
-   `required` - Set to `true` to make this option a required one.
-   `takes_value` - Set to `true` to specify that this option can have a value.
-   `multiple` - If set to `true` the option may be used multiple times.
-   `global` - This argument is possible for all subcommands.
-   `empty_values` - Set to `false` to disallow empty values.
-   `hidden` - Set to `true` to hide this argument from help page.
-   `possible_values` - Only the given list of values is possible.
-   `case_insensitive` - This option can be called case insensitive.
-   `group` - Combine parameters within named groups.
-   `number_of_values` - Specify how many values to be given to this argument.
-   `max_values` - Maximum values to be given.
-   `min_values` - Minimum values to be given.
-   `validator` - Add a custom validator method.
-   `default_value` - Value used for this argument if nothing is given.
-   `default_value_if` - If given option name is equal to value the given value is used as default.
-   `env` - Specifies that if the value is not passed in as an argument, that it should be retrieved from the environment, if available.
-   `value_name` - Name used for value in help information.
-   `index` - Set a index position for ordered arguments

Relational methods:

-   `required_unless` - Given an option name which makes this option required if not set.
-   `required_unless_all` - All given option names (as list) have to be set or this option is required.
-   `required_unless_one` - At least one option of the list has to be set or this option is required.
-   `conflicts_with` - If this argument is set the given option name is not allowed.
-   `conflicts_with_all` - Set multiple conflicting options.
-   `overrides_with` - If the defined option name is present this argument will be removed.
-   `overrides_with_all` - Set multiple overriding options.
-   `requires` - The named option is required if this argument is given.
-   `requires_if` - The named option is required if this argument equals the given value.
-   `required_if` - This argument is required if the named option is equal the given value.

## Sub Command

Using `subcomman` method you can add such with it's own arguments, subcommands, version, usage, etc. They also function just like the main app.

```rust
clap::SubCommand::with_name("config")
    .about("Used for configuration")
    .arg(Arg::with_name("config_file")
        .help("The configuration file to use")
        .index(1)))
```

The methods here are the same as for the main app.

## Working with Values

Using the method `get_matches` you get information about the arguments that where supplied to the program at runtime by the user.

```rust
let args = clap::App::new(crate_name!())
    .version(crate_version!())
    .about(crate_description!())
    .author(crate_authors!())
    .arg(clap::Arg::with_name("config")
        .short("c")
        .long("config")
        .takes_value(true)
        .value_name("FILE")
        .help("Provides a config file to myprog")
    )
    .get_matches();
```

The matches structure, here stored in `args` can now be asked for. All methods work with the name of the options given in the arguments definition.

-   `value_of` - Gets the value of a specific option or positional argument. If the option wasn't present at runtime it returns `None`.
-   `values_of` - Does the same but for options which may occur multiple times and returns an `Iterator`.
-   `is_present` - Returns true if an argument was present at runtime, otherwise false.
-   `occurences_of` - Returns the number of times that argument is given.
-   `index_of` - Gets the starting index of the argument in respect to all other arguments.
-   `indices_of` - Does the same for multiple arguments.
-   `subcommand_name` - Returns the name of the subcommand that was used at runtime, or `None` if one wasn't.
-   `subcommand_matches` - Get the arguments specified for the subcommand.

## Examples

Check for present argument:

```rust
if args.is_present("list") {
    println!("Printing testing lists...");
} else {
    println!("Not printing testing lists...");
}
```

Check how many times an argument is given:

```rust
match args.occurrences_of("v") {
    0 => println!("Verbose mode is off"),
    1 => println!("Print only some information"),
    2 => println!("Print a lot of information"),
    3 | _ => println!("Going crazy"),
}
```

Check for the existence of subcommands, and if found use their matches just as you would the top level app:

```rust
if let Some(matches) = args.subcommand_matches("test") {
    // "$ myapp test" was run
    if matches.is_present("list") {
        // "$ myapp test -l" was run
        println!("Printing testing lists...");
    } else {
        println!("Not printing testing lists...");
    }
}
```

## Shell Completion

Completion scripts can be created by clap on call. Therefore a subcommand to do this is needed:

```rust
.subcommand(SubCommand::with_name("completions")
    .about("Generates completion scripts for your shell")
    .arg(Arg::with_name("SHELL")
        .required(true)
        .possible_values(&["bash", "fish", "zsh"])
        .help("The shell to generate the script for")))
```

And the following code will generate it:

```rust
match args.subcommand() {
    ("completions", Some(sub_args)) => {
        let shell = sub_args.value_of("SHELL").unwrap();
        cli().gen_completions_to(
            crate_name!(),
            shell.parse::<clap::Shell>().unwrap(),
            &mut io::stdout(),
        );
    }
    (_, _) => unimplemented!(), // for brevity
}
```
