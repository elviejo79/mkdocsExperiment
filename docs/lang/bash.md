# Bash

The `bash` shell is the most common shell in Linux/Unix systems and also called the GNU shell. It is a superset of the `sh` shell, so everything that works there also works in `bash`.

But bash is not only a shell, it is also a powerful scripting language but for the higher level techniques a good understanding of the system and the language is needed. With complex data structures it gets a little cryptic as long as you don't know the syntax.

Here the language structure and special use cases will be explained.

## Script vs Shell

Everything which is possible in a script can also be directly typed within the shell. That allows to debug a code by running each line one by one in the console.

When the program being executed is a shell script, bash will create a new bash process using a fork. This sub shell reads the lines from the shell script one line at a time. Commands on each line are read, interpreted and executed as if they would have come directly from the keyboard.
Don't change the file contents while a script is running, because this can influence the running program and tend to incalculable results.

## Syntax

Comments: use `#` to start a comment

Commands: calls the function, built-in or within the `PATH`

Arguments: separated by spaces behind the command

Exits Status: is the integer return value which is `0` for success: [exit codes](../concepts/exitcodes.md)

Shebang: `#!/bin/bash` as first line is used to run script as `bash` if executed

## Redirection and Pipes

There are three file descriptors: `STDIN` (0), `STDOUT` (1) and `STDERR` (2) which can be used.
They can be directed into a file or piped as `STDIN` to the next command.

| Redirection                   | Code Example                            |
| ----------------------------- | --------------------------------------- |
| `STDOUT` to file              | `ls -l > ls-l.txt`                      |
| `STDERR` to file              | `grep da * 2> grep-errors.txt`          |
| `STDOUT` to `STDERR`          | `grep da * 1>&2`                        |
| `STDERR` to `STDOUT`          | `grep * 2>&1`                           |
| `STDOUT` and `STDERR` to file | `grep da * &> grep-errors.txt`          |
| pipe `STDOUT`                 | `ls -l | grep "\.txt$"`                 |
| pipe `STDOUT` and `STDERR`    | `ls -l |& grep "\.txt$"`                |
| pipe only `STDERR`            | `ls -l >/dev/null 2>&1 | grep "\.txt$"` |

## Variables

A variable is created by setting a value to it like `test="xxxx"`.
They are only accessible in the current shell. To make them also available to sub shells you have to `export` it.

The following types of variables are possible:

-   String variables
-   Integer variables
-   Constant variables
-   Array variables

Within functions local variables can be defined by using the `local` prefix.

The following variables have special use cases:

| Variable          | Usage                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `$*`              | Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, it expands to a single word with the value of each parameter separated by the first character of the IFS special variable.                                                                                                                                                                                                        |
| `$@`              | Expands to the positional parameters, starting from one. When the expansion occurs within double quotes, each parameter expands to a separate word.                                                                                                                                                                                                                                                                                        |
| `$#`              | Expands to the number of positional parameters in decimal.                                                                                                                                                                                                                                                                                                                                                                                 |
| `$?`              | Expands to the exit status of the most recently executed foreground pipeline.                                                                                                                                                                                                                                                                                                                                                              |
| `$-`              | A hyphen expands to the current option flags as specified upon invocation, by the set built-in command, or those set by the shell itself (such as the -i).                                                                                                                                                                                                                                                                                 |
| `$$`              | Expands to the process ID of the shell.                                                                                                                                                                                                                                                                                                                                                                                                    |
| `$!`              | Expands to the process ID of the most recently executed background (asynchronous) command.                                                                                                                                                                                                                                                                                                                                                 |
| `$0`              | Expands to the name of the shell or shell script.                                                                                                                                                                                                                                                                                                                                                                                          |
| `$_`              | The underscore variable is set at shell startup and contains the absolute file name of the shell or script being executed as passed in the argument list. Subsequently, it expands to the last argument to the previous command, after expansion. It is also set to the full pathname of each command executed and placed in the environment exported to that command. When checking mail, this parameter holds the name of the mail file. |
| `CDPATH`          | A colon-separated list of directories used as a search path for the `cd` command.                                                                                                                                                                                                                                                                                                                                                          |
| `HOME`            | The current user's home directory; the default for `cd`. The value of this variable is also used by tilde expansion.                                                                                                                                                                                                                                                                                                                       |
| `IFS`             | A list of characters that separate fields; used when the shell splits words as part of expansion.                                                                                                                                                                                                                                                                                                                                          |
| `MAIL`            | If this parameter is set to a file name and the `MAILPATH` variable is not set, Bash informs the user of the arrival of mail in the specified file.                                                                                                                                                                                                                                                                                        |
| `MAILPATH`        | A colon-separated list of file names which the shell periodically checks for new mail.                                                                                                                                                                                                                                                                                                                                                     |
| `OPTARG`          | The value of the last option argument processed by `getopts`.                                                                                                                                                                                                                                                                                                                                                                              |
| `OPTIND`          | The index of the last option argument processed by `getopts`.                                                                                                                                                                                                                                                                                                                                                                              |
| `PATH`            | A colon-separated list of directories in which the shell looks for commands.                                                                                                                                                                                                                                                                                                                                                               |
| `PS1`             | The primary prompt string. The default value is `'\s-\v\$ '`.                                                                                                                                                                                                                                                                                                                                                                              |
| `PS2`             | The secondary prompt string. The default value is `'> '`.                                                                                                                                                                                                                                                                                                                                                                                  |
| `auto_resume`     | This variable controls how the shell interacts with the user and job control.                                                                                                                                                                                                                                                                                                                                                              |
| `BASH`            | The full pathname used to execute the current instance of Bash.                                                                                                                                                                                                                                                                                                                                                                            |
| `BASH_ENV`        | If this variable is set when Bash is invoked to execute a shell script, its value is expanded and used as the name of a startup file to read before executing the script.                                                                                                                                                                                                                                                                  |
| `BASH_VERSION`    | The version number of the current instance of Bash.                                                                                                                                                                                                                                                                                                                                                                                        |
| `BASH_VERSINFO`   | A read-only array variable whose members hold version information for this instance of Bash.                                                                                                                                                                                                                                                                                                                                               |
| `COLUMNS`         | Used by the select built-in to determine the terminal width when printing selection lists.                                                                                                                                                                                                                                                                                                                                                 |
| `COMP_CWORD`      | An index of the word containing the current cursor position.                                                                                                                                                                                                                                                                                                                                                                               |
| `COMP_LINE`       | The current command line.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `COMP_POINT`      | The index of the current cursor position relative to the beginning of the current command.                                                                                                                                                                                                                                                                                                                                                 |
| `COMP_WORDS`      | An array variable consisting of the individual words in the current command line.                                                                                                                                                                                                                                                                                                                                                          |
| `COMPREPLY`       | An array variable from which Bash reads the possible completions generated by a shell function invoked by the programmable completion facility.                                                                                                                                                                                                                                                                                            |
| `DIRSTACK`        | An array variable containing the current contents of the directory stack.                                                                                                                                                                                                                                                                                                                                                                  |
| `EUID`            | The numeric effective user ID of the current user.                                                                                                                                                                                                                                                                                                                                                                                         |
| `FCEDIT`          | The editor used as a default by the -e option to the `fc` command.                                                                                                                                                                                                                                                                                                                                                                         |
| `FIGNORE`         | A colon-separated list of suffixes to ignore when performing file name completion.                                                                                                                                                                                                                                                                                                                                                         |
| `FUNCNAME`        | The name of any currently-executing shell function.                                                                                                                                                                                                                                                                                                                                                                                        |
| `GLOBIGNORE`      | A colon-separated list of patterns defining the set of file names to be ignored by file name expansion.                                                                                                                                                                                                                                                                                                                                    |
| `GROUPS`          | An array variable containing the list of groups of which the current user is a member.                                                                                                                                                                                                                                                                                                                                                     |
| `histchars`       | Up to three characters which control history expansion, quick substitution, and tokenization.                                                                                                                                                                                                                                                                                                                                              |
| `HISTCMD`         | The history number, or index in the history list, of the current command.                                                                                                                                                                                                                                                                                                                                                                  |
| `HISTCONTROL`     | Defines whether a command is added to the history file.                                                                                                                                                                                                                                                                                                                                                                                    |
| `HISTFILE`        | The name of the file to which the command history is saved. The default value is `~/.bash_history`.                                                                                                                                                                                                                                                                                                                                        |
| `HISTFILESIZE`    | The maximum number of lines contained in the history file, defaults to 500.                                                                                                                                                                                                                                                                                                                                                                |
| `HISTIGNORE`      | A colon-separated list of patterns used to decide which command lines should be saved in the history list.                                                                                                                                                                                                                                                                                                                                 |
| `HISTSIZE`        | The maximum number of commands to remember on the history list, default is 500.                                                                                                                                                                                                                                                                                                                                                            |
| `HOSTFILE`        | Contains the name of a file in the same format as /etc/hosts that should be read when the shell needs to complete a hostname.                                                                                                                                                                                                                                                                                                              |
| `HOSTNAME`        | The name of the current host.                                                                                                                                                                                                                                                                                                                                                                                                              |
| `HOSTTYPE`        | A string describing the machine Bash is running on.                                                                                                                                                                                                                                                                                                                                                                                        |
| `IGNOREEOF`       | Controls the action of the shell on receipt of an EOF character as the sole input.                                                                                                                                                                                                                                                                                                                                                         |
| `INPUTRC`         | The name of the Readline initialization file, overriding the default `/etc/inputrc`.                                                                                                                                                                                                                                                                                                                                                       |
| `LANG`            | Used to determine the locale category for any category not specifically selected with a variable starting with `LC_`.                                                                                                                                                                                                                                                                                                                      |
| `LC_ALL`          | This variable overrides the value of `LANG` and any other `LC_` variable specifying a locale category.                                                                                                                                                                                                                                                                                                                                     |
| `LC_COLLATE`      | This variable determines the collation order used when sorting the results.                                                                                                                                                                                                                                                                                                                                                                |
| `LC_CTYPE`        | This variable determines the interpretation of characters and the behavior of character classes within file name expansion and pattern matching.                                                                                                                                                                                                                                                                                           |
| `LC_MESSAGES`     | This variable determines the locale used to translate double-quoted strings preceded by a "$" sign.                                                                                                                                                                                                                                                                                                                                        |
| `LC_NUMERIC`      | This variable determines the locale category used for number formatting.                                                                                                                                                                                                                                                                                                                                                                   |
| `LINENO`          | The line number in the script or shell function currently executing.                                                                                                                                                                                                                                                                                                                                                                       |
| `LINES`           | Used by the select built-in to determine the column length for printing selection lists.                                                                                                                                                                                                                                                                                                                                                   |
| `MACHTYPE`        | A string that fully describes the system type on which Bash is executing, in the standard GNU CPU-COMPANY-SYSTEM format.                                                                                                                                                                                                                                                                                                                   |
| `MAILCHECK`       | How often (in seconds) that the shell should check for mail in the files specified in the `MAILPATH` or `MAIL` variables.                                                                                                                                                                                                                                                                                                                  |
| `OLDPWD`          | The previous working directory as set by the `cd` built-in.                                                                                                                                                                                                                                                                                                                                                                                |
| `OPTERR`          | If set to the value 1, Bash displays error messages generated by getopts.                                                                                                                                                                                                                                                                                                                                                                  |
| `OSTYPE`          | A string describing the operating system Bash is running on.                                                                                                                                                                                                                                                                                                                                                                               |
| `PIPESTATUS`      | An array variable containing a list of exit status values from the processes in the most recently executed foreground pipeline.                                                                                                                                                                                                                                                                                                            |
| `POSIXLY_CORRECT` | If this variable is in the environment when bash starts, the shell enters POSIX mode.                                                                                                                                                                                                                                                                                                                                                      |
| `PPID`            | The process ID of the shell's parent process.                                                                                                                                                                                                                                                                                                                                                                                              |
| `PROMPT_COMMAND`  | If set, the value is interpreted as a command to execute before the printing of each primary prompt (PS1).                                                                                                                                                                                                                                                                                                                                 |
| `PS3`             | The value of this variable is used as the prompt for the select command. Defaults to `'#? '`                                                                                                                                                                                                                                                                                                                                               |
| `PS4`             | The value is the prompt printed before the command line is echoed when the -x option is set; defaults to `'+ '`.                                                                                                                                                                                                                                                                                                                           |
| `PWD`             | The current working directory as set by the `cd` command.                                                                                                                                                                                                                                                                                                                                                                                  |
| `RANDOM`          | Each time this parameter is referenced, a random integer between 0 and 32767 is generated. Assigning a value to this variable seeds the random number generator.                                                                                                                                                                                                                                                                           |
| `REPLY`           | The default variable for the read built-in.                                                                                                                                                                                                                                                                                                                                                                                                |
| `SECONDS`         | This variable expands to the number of seconds since the shell was started.                                                                                                                                                                                                                                                                                                                                                                |
| `SHELLOPTS`       | A colon-separated list of enabled shell options.                                                                                                                                                                                                                                                                                                                                                                                           |
| `SHLVL`           | Incremented by one each time a new instance of Bash is started.                                                                                                                                                                                                                                                                                                                                                                            |
| `TIMEFORMAT`      | The value of this parameter is used as a format string specifying how the timing information for pipelines prefixed with the time reserved word should be displayed.                                                                                                                                                                                                                                                                       |
| `TMOUT`           | If set to a value greater than zero, `TMOUT` is treated as the default timeout for the read built-in. In an interative shell, the value is interpreted as the number of seconds to wait for input after issuing the primary prompt when the shell is interactive.                                                                                                                                                                          |
| `UID`             | The numeric, real user ID of the current user.                                                                                                                                                                                                                                                                                                                                                                                             |

## Functions

To define functions two types are possible:

```bash
function quit {
    exit
}

hello () {
    echo Hello!
}

hello
quit
```

Parameters given to the functions are readable as `$1`... like from any command.

## Expansion

**Brace expansion** is a mechanism by which arbitrary strings may be generated. Patterns to be brace-expanded take the form of an optional PREAMBLE, followed by a series of comma-separated strings between a pair of braces, followed by an optional POSTSCRIPT. The preamble is prefixed to each string contained within the braces, and the postscript is then appended to each resulting string, expanding left to right.

Brace expansions may be nested. The results of each expanded string are not sorted; left to right order is preserved:

```bash
$ echo sp{el,il,al}l
spell spill spall
```

**Variable expansion** will replace a variable with it's content, 

**Command substitution** will replace a command with it's output before evaluating the outer line.

```bash
$(command)
# backticks alternative
`command` 
```

**Arithmetic expansion** allows the evaluation of an arithmetic expression and the substitution of the result. The format for arithmetic expansion is:

```bash
$(( EXPRESSION ))
```

The expression is treated as if it were within double quotes. All tokens in the expression undergo parameter expansion, command substitution, and quote removal. Arithmetic substitutions may be nested.

Evaluation of arithmetic expressions is done in fixed-width integers with no check for overflow - although division by zero is trapped and recognized as an error. The operators are roughly the same as in the C programming language. In order of decreasing precedence, the list looks like this:

| Operator                                                             | Meaning                                    |
| -------------------------------------------------------------------- | ------------------------------------------ |
| `VAR++` and `VAR--`                                                  | variable post-increment and post-decrement |
| `++VAR` and `--VAR`                                                  | variable pre-increment and pre-decrement   |
| `-` and `+`                                                          | unary minus and plus                       |
| `!` and `~`                                                          | logical and bitwise negation               |
| `**`                                                                 | exponentiation                             |
| `*`, `/` and `%`                                                     | multiplication, division, remainder        |
| `+` and `-`                                                          | addition, subtraction                      |
| `<<` and `>>`                                                        | left and right bitwise shifts              |
| `<=`, `>=`, `<` and `>`                                              | comparison operators                       |
| `==` and `!=`                                                        | equality and inequality                    |
| `&`                                                                  | bitwise AND                                |
| `^`                                                                  | bitwise exclusive OR                       |
| `|`                                                                  | bitwise OR                                 |
| `&&`                                                                 | logical AND                                |
| `||`                                                                 | logical OR                                 |
| `expr ? expr : expr`                                                 | conditional evaluation                     |
| `=`, `*=`, `/=`, `%=`, `+=`, `-=`, `<<=`, `>>=`, `&=`, `^=` and `|=` | assignments                                |  | assignments |
| `,`                                                                  | separator between expressions              |


**File name expansion** is done using `*`, `?`, and `[`. If one of these characters appears, then the word is regarded as a `PATTERN`, and replaced with an alphabetically sorted list of file names matching the pattern. If no matching file names are found, and the shell option `nullglob` is disabled, the word is left unchanged. 

## Conditionals

```bash
if [ "foo" = "foo" ]; then
    echo expression evaluated as true
else
    echo expression evaluated as false
fi
```

## Test Conditions

In Bash there are different extensions to write conditions:

1. `[ ... ]` is POSIX command
2. `[[ ... ]]` is a Bash extension

`[[ X ]]` is a single construct that makes `X` be parsed magically. `<`, `&&`, `||` and `()` are treated specially, and word splitting rules are different. There are also further differences like `=` and `=~`.

| Type                         | POSIX                                                                                       | Bash                                     |
| ---------------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------- |
| `<`                          | `[ a \< b ]` comparison needs `\` <br>else redirects output                                 | `[[ a < b ]]`                            |
| `&&` or `||`                 | `[ a = a ] && [ b = b ]` <br>deprecated: `[ a = a -a b = b ]`                               | `[[ a = a && b = b ]]`                   |
| `(`                          | `([ a = a ] || [ a = b ]) && [ a = b ]` <br>deprecated: `[ \( a = a -o a = b \) -a a = b ]` | `[[ (a = a || a = b) && a = b ]]`        |
| word splitting <br>`x='a b'` | `[ "$x" = 'a b' ]` <br>quotes needed to prevent expand                                      | `[[ $x = 'a b' ]]`                       |
| `=` or `==`                  | `printf 'ab' | grep -Eq 'a.'`                                                               | `[[ ab = a? ]]` pattern matching applies |
| `=~` or `!~`                 | `printf 'ab' | grep -Eq 'ab?'`                                                              | `[[ ab =~ ab? ]]` POSIX regexp           |


As you see the bash expressions are often better readable.

## Regular Expressions

| Operator | Effect                                                                                                          |
| -------- | --------------------------------------------------------------------------------------------------------------- |
| .        | Matches any single character.                                                                                   |
| ?        | The preceding item is optional and will be matched, at most, once.                                              |
| *        | The preceding item will be matched zero or more times.                                                          |
| +        | The preceding item will be matched one or more times.                                                           |
| {N}      | The preceding item is matched exactly N times.                                                                  |
| {N,}     | The preceding item is matched N or more times.                                                                  |
| {N,M}    | The preceding item is matched at least N times, but not more than M times.                                      |
| -        | represents the range if it's not first or last in a list or the ending point of a range in a list.              |
| ^        | Matches the empty string at the beginning of a line; also represents the characters not in the range of a list. |
| $        | Matches the empty string at the end of a line.                                                                  |
| \b       | Matches the empty string at the edge of a word.                                                                 |
| \B       | Matches the empty string provided it's not at the edge of a word.                                               |
| \<       | Match the empty string at the beginning of word.                                                                |

## Loops

Bash has three different kind of loops:

1. The `for` loop is a little bit different from other programming languages. Basically, it let's you iterate over a series of 'words' within a string.
2. The `while` executes a piece of code if the control expression is `true`, and only stops when it is `false`.
3. The `until` loop is almost equal to the `while` loop, except that the code is executed while the control expression evaluates to `false`.

Each loop may be exited using `break` or continued with the next round using `continue`.

```bash
for i in $( ls ); do
    echo item: $i
done
```

The variable `i` will take the different values contained in `$( ls )`.
The block enclosed in `do` and `done` is executed in each iteration.

To use `for` as a counter use the range support of bash:

```bash
for i in {1..10}; do
    echo $i
done
```

But you can also use a C-like for loop:

```bash
for (( c=1; c<=5; c++ )); do  
   echo "Welcome $c times"
done
```

In `while` and `until` you give the condition at the start and use a code block like used in `for`:

```bash
COUNTER=0
while [  $COUNTER -lt 10 ]; do
    echo The counter is $COUNTER
    let COUNTER+=1 
done
```

```bash
COUNTER=20
until [  $COUNTER -lt 10 ]; do
    echo COUNTER $COUNTER
    let COUNTER-=1
done
```

## Options

Shell options can either be set different from the default upon calling the shell, or be set during shell operation. 
To change an option use `set -<option>`to enable or `set +<option>` to disable an option.

| Option      | Usage                                                                    |
| ----------- | ------------------------------------------------------------------------ |
| `noclobber` | prevents existing files from being overwritten by redirection operations |
| `noglob`    | prevents special characters from being expanded                          |
| `u`         | treat unset variables as an error                                        |
| `x`         | set debug mode which will print each expanded command before executing   |

## Include modules

Using the `source <path>` command the given script will be included like it is directly written there the `source` command is set.

A relative path is based on the current directory of the calling shell.



{!docs/abbreviations.txt!}

