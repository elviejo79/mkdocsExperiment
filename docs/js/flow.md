title: Flow

# Flow Annotated JS

[Flow](https://flow.org) is a static type checker which checks your code for errors
through static type annotations. These types allows to tell Flow how you want your
code to work, and Flow will make sure it does work that way.

It is an alternative against [TypeScript](typescript.md).

```js
// @flow
function square(n: number): number {
    return n * n; // normally the error is here
}

square("2"); // with flow it is reported here
```

## Installing Flow

This can be setup as extension to babel which will strip of the flow definition on
transpiling:

```bash
npm install --save-dev flow-bin flow-typed babel-preset-flow
npm run flow init
sudo npm install flow-bin # needed for some IDEs
node_modules/.bin/flow-typed install # will download your libraries
```

Also you have to add the preset `flow` to babel. Now the compiler will strip off
all flow extensions and the builded code will work as if it was never there.

After adding new modules you should update your flow-typed libraries:

```bash
node_modules/.bin/flow-typed install --overwrite
```

## Using Flow

To take advantage of the flow checking you run it using:

```bash
npm run flow
# or if installed globally
flow
```

This command may be run automatically from the `test` or `dev` task. And some editors
like Atom can directly validate and show problems while editing, now.

You may also have a look at some
[Examples](https://github.com/raquo/facebook-flow-examples/tree/master/examples)
from the net.

## Flow annotations

You have to start each file with `// @flow` as the first comment line.

### Classes

#### Declare properties

It is necessary to define each used property. This not only helps in type analyzation
but also to make it more readable:

```js
// @flow
class SchemaAny {
    data: any;

    constructor(data: any) {
        this.data = data;
    }
}
```

#### Command concatenation

If this should be done, don't define the return type as the current class. Better
use `this` as return type which will also work if a subclass calls methods from
it's super class:

```js
/* @flow */

class A {
    x(): this {
        return this;
    }
}

class B extends A {
    y(): this {
        return this;
    }
}

var w = new B();

w.x().y();
```

#### Uncheck by casting with any

It's not a very nice solution, but you can cast an object to any. And no unsafe any will be leaked.

```js
/* @flow */

class A {
    get clone(): this {
        //    return Object.assign(Object.create(this), this) // flow error
        return Object.assign((Object.create(this): any), this);
    }
}

class B extends A {}

var b = new B();
var c = b.clone;
```

Here the `Object.create(this)` will normally give an error: Co variant property clone incompatible
with contra-variant use in call of method assign.

The fix is to cast it by any.


{!docs/abbreviations.txt!}

