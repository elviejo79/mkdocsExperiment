title: Markdown

# Markdown Syntax

The easiest way to document is to use markdown syntax. You write the code in plain
text with some easy formatting rules. This can be transformed into HTML or other
formats. But it's not an inherent standard, different flavors exist.

For the alinex modules I decided to use this as the base for documentation.
The following examples should help to use the right syntax which will be
interpreted by GitHub and the integrated document generation of the
[alinex-builder](http://alinex.github.io/node-builder).

## Headings

Two formats of headers are supported. First you may use double or single underlines:

```text
Heading Level 1
==========================================

Heading Level 2
------------------------------------------
```

Or you can define the headings with starting hash signs:

```text
# Heading Level 1

## Heading Level 2

### Heading Level 4

#### Heading Level 3

##### Heading Level 5

###### Heading Level 6
```

## Paragraphs

Paragraphs are written with an empty line. Newlines are ignored but you may add an
`<br />` Tag if you want:

```text
This text
will be in one line
<br>And this in a new one.
```

becomes

This text
will be in one line
<br>And this in a new one.

## Text Format

This styles can be used inline and also within a word. To prevent wrong
interpretation you may escape the signs with an backslash `\`.

### Strikethrough

**Currently not supported in the document generation but in GitHub view!**

Use a double tilde sign before and after the text:

```text
Here is the ~~wrong text~~ right text.
```

becomes

Here is the ~~wrong text~~ right text.

### Emphasis

```text
*single asterisks*

_single underscores_
```

becomes

_single asterisks_

_single underscores_

### Strong

```text
**double asterisks**

__double underscores__
```

becomes

**double asterisks**

**double underscores**

### Code

```text
`code in backquotes`
```

becomes

```text
`code in backquotes`
```

## Links

If you give a standard URL in the text an link will be added automatically. You
can also surround them with `<...>`:

```text
http://example.com and <http://example.com>

info@alinex.de and <info@alinex.de>
```

becomes

http://example.com and <http://example.com>

info@alinex.de and <mailto:info@alinex.de>

You may also give a special link text by writing the text in square brackets and
the link in round brackets behind:

```text
This is the alinex [home](http://alinex.github.io/node-alinex).
```

becomes

This is the alinex [home](http://alinex.github.io/node-alinex).

And at last you may use referenced links like that

```text
This is [an example][id] reference-style link.

[id]: http://example.com/  "Optional Title Here"
```

becomes

This is [an example][id] reference-style link.

[id]: http://example.com/ "Optional Title Here"

The referenced links will get you the same output but makes the source code
more readable. You may also keep the id empty to use the name also as the id.

## Lists

You can write unordered lists like:

```text
* red
* green
* blue
```

which becomes

- red
- green
- blue

You can do the exactly same with an `*`, `+` or `-` sign.

Ordered lists are written with the numbers following by periods:

```text
1. red
2. green
3. blue
```

will become

1.  red
2.  green
3.  blue

If your list entry has multiple paragraph the second paragraph has to be indented
by 4 spaces.
To avoid interpreting number with dots as list you may escape the dot with an
backslash.

## Blockquotes

You may use email like block indenting with lines beginning with one or
multiple `>`. You can also be lazy and put the sign only on the first line of an
paragraph.

```text
I answered

> Hello
>
> > Is this code written in coffeescript?
>
> Thats correct.
```

becomes

I answered

> Hello
>
> > Is this code written in coffeescript?
>
> Thats correct.

Blockquotes can also contain any other markdown format.

## Code blocks

Standard Markup Syntax is to indent code blocks with four spaces:

```text
function test() {
  console.log('Test succeeded!');
}
```

A better way is to fence the code like below. This also adds the ability to
specify the language used (needed for syntax highlighting):

    ``` javascript
    function test() {
      console.log('Test succeeded!');
    }
    ```

    ``` coffee
    test = ->
      console.log 'Test succeeded!'
    ```

becomes

```javascript
function test() {
  console.log("Test succeeded!");
}
```

```coffee
test = ->
  console.log 'Test succeeded!'
```

Possible languages are listed in the
[languages YAML file](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml).

## Horizontal lines

You can add them by writing a paragraph which consists of only three or more
`*` or `-` signs:

```text
----------------------
```

becomes

---

## Tables

**Currently not supported in the document generation but in GitHub view!**

Tables will be created by dividing cells with a pipe `|` and the header row with
hyphens `-`:

```text
    | Color | Value  |
    | ----- | ------ |
    | red   | ff0000 |
    | green | 00ff00 |
    | blue  | 0000ff |
```

becomes

| Color | Value  |
| ----- | ------ |
| red   | ff0000 |
| green | 00ff00 |
| blue  | 0000ff |

The cells don't need the exact same width it may also vary in the same column.
And you may put inline markup into the cell like `bold`, `italic`...

To align columns you have to add a colon in the divider line between header and
body:

```text
| Left               |       Center       |              Right |
| :----------------- | :----------------: | -----------------: |
| first entry        |    first entry     |        first entry |
| another            |      another       |            another |
| and the last entry | and the last entry | and the last entry |
```

becomes

| Left               |       Center       |              Right |
| :----------------- | :----------------: | -----------------: |
| first entry        |    first entry     |        first entry |
| another            |      another       |            another |
| and the last entry | and the last entry | and the last entry |

## Symbols & HTML

You may also use some HTML symbols or tags but only as less as possible:

```text
&copy;<br />&frac12;
```

becomes

    &copy;<br />&frac12;

## Images

Images may be placed like links but start with an exclamation mark `!`:

```text
![Alinex Logo](../alinex-logo.png "ALINEX")
```

becomes

![Alinex Logo](../assets/alinex-logo.png "ALINEX")

The title at the end is optional but will output as the tooltip.

Like for links you may use the reference style here, too:

```text
![Alinex Logo][id]

[id]: ../alinex-logo.png "ALINEX"
```

becomes

![Alinex Logo][id]

[id]: ../assets/alinex-logo.png "ALINEX"

## Extended Markdown

Within the [report](http://alinex.github.io/node-report) package an extended
version of markdown conversion is used supporting all of the above but also

- boxes
- emoji
- special styles
- table of contents
- footnotes
- acronyms
- and more


{!docs/abbreviations.txt!}

