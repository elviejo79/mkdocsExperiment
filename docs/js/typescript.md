# TypeScript

TypeScript is a superset of JavaScript which primarily provides optional static typing, classes and interfaces. One of the big benefits is to enable IDEs to provide a richer environment for spotting common errors as you type the code.

Instead of only adding types to standard JavaScript like flow does TypeScript is a
language of its own. It is aimed for large projects to get more robust software,
while still being deployable as regular JavaScript.

I for myself find TypeScript better supported, better community and faster developing
so after my first steps with flow+babel I switched to TypeScript.

## Installation

First you need to install TypeScript:

```bash
npm install typescript ts-node @types/node --save-dev
```

To make it usable you also have to add a configuration file `tsconfig.json` in your
project root:

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "dist",
    "declaration": true,
    "sourceMap": true,
    "removeComments": true,
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  },
  "files": [
    "./node_modules/@types/mocha/index.d.ts",
    "./node_modules/@types/node/index.d.ts"
  ],
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Usage

Now you can integrate it in your `package.json`:

```json
{
  "scripts": {
    "lint": "tslint -c tslint.json 'src/**/*.ts'",
    "test": "mocha -r ts-node/register test/mocha/**.ts",
    "coverage": "nyc mocha",
    "dev":
      "NODE_ENV=development nodemon --exec ./node_modules/.bin/ts-node -- ./src/index.ts",
    "build": "tsc",
    "start": "node dist/index",
    "prepublish": "npm run build"
  },
  "engines": {
    "node": ">=6"
  }
}
```

Also specify the engines information to help people use your module in the right version.

This defines the transformation to:

1.  Be used as JIT compiler in dev mode (run using `npm run dev`)
2.  Be converted into ES6 code by running `npm run build` (into `dist` folder)
3.  Everything in dist folder can be used without TypeScript
4.  Before publishing to npm the `build` command is called automatically

## Test

In testing typescript private members and functions are not accessible. You will
get a compile error. Netherless it will be accessible in the generated JavaScript
at the moment.

But for testing it is often needed to access them. This should be done by changing
the source code without really making the generated code complexer. To do this
I implemented it in the following solution.

Use the access modifiers as follows:

- public - completely accessible
- protected - things which should be available in extended classes or test
- private - something you only need in the class

As you see I use the protected for something needed in test like:

```js
class Work {
  protected load: integer
  constructor() {
    this.load = 1
  }
}
exports default Work
```

Now to access the `load` property I use a wrapper class within the test folder
(so it will not go into the dist):

```js
import * as Work from "../../src/Work"
class TWork extends Work {
  public getLoad(): integer {
    return this.load
  }
}
exports default TWork
```

Now within the test I use the added `getLoad()` method:

```js
import * TWork from "../wrapper/TWork"
const work = new TWork()
expect(work.getLoad()).to.equal(1)
```

The directory structure used looks like:

    src/Work.ts
    test/
      mocha/test.ts
      wrapper/TWork.ts
    dist/Work.js


{!docs/abbreviations.txt!}

