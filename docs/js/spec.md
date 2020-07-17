title: Specification

# JavaScript

JavaScript is a scripting language which grows out of the Netscape Browser. It
is now over 20 years old and since it started as a browser language in one browser
it grew to the ultimate web scripting language supported in all browsers. In the
last years the Node.JS project brought JavaScript to a general language usable
on the server or desktop.

The core is standardized as ECMAScript in different Versions. The language itself
was kept the same for some years but just now it develops further and get more
modern in ECMAScript6 or ECMAScript7. Read more on modern JavaScript under [ES.Next](es-next.md).

As of now, JavaScript can be used everywhere not only in the browser but also on the server using [Node.JS](../js/nodejs.md).

## Specification

JavaScript is an interpreter language with a C-like syntax. It is a dynamic typed
language in which variables point to values which are associated to types.
It is object driven in which most things are objects which are associative arrays
with prototypes. Prototypes are used for inheritance instead of classes. But class
based features could be simulated through prototypes, too.
The language is event driven and it's core is an event engine through which the
functions will be called. This gives the possibility of doing things in parallel
within one process while waiting for external data.

### Basics

**Statements**: Within JavaScript statements will end with a `;` or at the end of line (in most cases).

**Comments**: One liners with double slash `//` or multiline with slash and an asterisk `/*...*/`.

**Type Conversions**: By calling the Type Class with the value: `String(value)` also automatic type conversion will be used.

**DataTypes**: Number, String, Boolean, Object, Symbol

**Arrays**: Arrays are a special `Object`

**Variables**: Use `let` or `var` to declare them.

**String concatenation**: Use `+` to combine two strings into a new one.

**Comparisons**: Strict equal is done by `===` or negative by `!==`

**Tenary operator**: Supports also `?`

## Books

I can not give any recommendation because there are so much of them and I never really learned from an book but grew with the language and it's community. You can find them in book stores or also online.

If you know the language a reference like:

- [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [W3 School](https://www.w3schools.com/jS/default.asp)

And as always if you don't know how [StackOverflow](https://stackoverflow.com/questions/tagged/javascript) is your friend.


{!docs/abbreviations.txt!}

