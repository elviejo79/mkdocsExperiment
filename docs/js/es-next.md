title: ES.Next

# ES.Next JavaScript

ECMAScript is the standard upon which JavaScript is based, and it’s often abbreviated to ES.

ES5 was 10 years in the making, from 1999 to 2009, and as such it was also a fundamental and very important revision of the language, but now much time has passed that it’s not worth discussing how code before ES5 worked.

Since 2015 the official version names use the year of it's final release but in the community also the ongoing short number is also be used. So here I will show both. Before 2015 there the standard was more statically but with the defined yearly release cycles of the proposal this all changed.

The implementation is always way behind depending on the specific engine but by using transpilers most newer features may also be used nowadays.

## ECMAScript 2015 (ES6)

ES6 also known as ECMAScript 2015 adds significant new syntax for writing complex applications, including classes and modules. It contains a lot of new features because a long time passed between ES5 and ES6. The most important changes in ES2015 include:

-   Arrow functions
-   Promises
-   Iterators
-   Generators
-   `let` and `const`
-   Classes
-   Modules
-   Template literals
-   Default parameters
-   The spread operator
-   Destructuring assignments
-   Enhanced object literals
-   Map and Set

See a lot of them described [here](es2015.md)

## ECMAScript 2016 (ES7)

ES7 is a tiny release with only two new functionalities:

-   `Array.prototype.includes`
-   Exponentiation Operator

See a lot of them described [here](es2016.md)

## ECMAScript 2017 (ES8)

Compared to it's number of features this introduces only some but very useful features:

-   String padding
-   `Object.values`
-   `Object.entries`
-   `Object.getOwnPropertyDescriptors()`
-   Trailing commas in function parameter lists and calls
-   Async functions
-   Shared memory and atomics

See a lot of them described [here](es2017.md)

## ECMAScript 2018 (ES9)

Major new features:

-   Asynchronous Iteration
-   Rest/Spread Properties

New regular expression features:

-   RegExp named capture groups
-   RegExp Unicode Property Escapes
-   RegExp Look-behind Assertions
-   s (dotAll) flag for regular expressions

Other new features:

-   `Promise.prototype.finally()`
-   Template Literal Revision

See a lot of them described [here](es2018.md)

## ECMAScript 2019 (ES10)

New features:

-   `Array.flat()`
-   `Array.flatMap()`
-   `Object.fromEntries()`
-   `String.trimStart()` and `String.trimEnd()`
-   Optional Catch Binding
-   `Function.toString()`
-   `Symbol.description`
-   Well Formed `JSON.Stringify()`
-   `Array.sort()` Stability
-   JSON ⊂ ECMAScript (JSON Superset)

See a lot of them described [here](es2019.md)

## Proposals

All the other proposal not included in any current standards are defined in 5 stages:

-   Stage 0 - Straw man: just an idea, possible Babel plugin.
-   Stage 1 - Proposal: this is worth working on.
-   Stage 2 - Draft: initial spec.
-   Stage 3 - Candidate: complete spec and initial browser implementations.
-   Stage 4 - Finished: will be added to the next yearly release.

## Babel

[Babel](http://babeljs.io/) is a JavaScript compiler which will take modern JavaScript
code and transpile them into an older language using transformations and polyfills.
Essentially this makes the code runnable in an environment which didn't support
the newest JavaScript standards.

To see what babel will do try it out in the online [Babel REPL](https://babeljs.io/repl/).
It's an interactive online playground that shows you how babel will translate your
code using different presets. But you can also use the `babel-node` CLI which comes
with an REPL like node itself to run code interactively on your local machine.

For the Alinex modules this is used to already use the newest language features
while also get it working today.

### Installing Babel

First you need to install Babel CLI within your project as development requirement
and you will also need some additional presets:

First you have to decide what babel should transform. The rules defines a range of possibilities
which are transformed down till a specific version to run.

#### Node 6 with support till stage-3

```bash
npm install babel-cli babel-preset-env babel-preset-stage-3 babel-polyfill --save-dev
```

To make it usable you also have to add a configuration file `.babelrc` in your project
root and specify the installed presets to be used:

```json
{
    "presets": [["env", { "targets": { "node": 6 } }], "stage-3"],
    "plugins": [],
    "comments": false
}
```

#### Older NodeJS support all proposals

```bash
npm install babel-cli babel-preset-es2015 babel-preset-stage-0 babel-polyfill --save-dev
```

To make it usable you also have to add a configuration file `.babelrc` in your project
root and specify the installed presets to be used:

```json
{
    "presets": ["es2015", "stage-0"],
    "plugins": [],
    "comments": false
}
```

### Using Babel

Now you can integrate babel with your project by defining it's call in the scripts
section of `package.json`:

```json
{
    "scripts": {
        "dev": "node_modules/.bin/babel-node src/index.js --require babel-polyfill",
        "build": "babel src -d dist --require babel-polyfill",
        "start": "node dist/index.js",
        "prepublish": "npm run build"
    },
    "engines": {
        "node": ">=6"
    }
}
```

Also specify the engines information to help people use your module in the right version.

This defines the babel transformation to:

1.  Be used as JIT compiler in development mode (run using `npm run dev`)
2.  Be converted into ES5 code by running `npm run build` (into `dist` folder)
3.  Everything in dist folder can be used without babel
4.  Before publishing to npm the `build` command is called automatically

The babel polyfills are needed to use the async/await which will come with
node 7.6.

{!docs/abbreviations.txt!}
