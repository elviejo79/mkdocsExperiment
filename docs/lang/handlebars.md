title: Handlebars

# Handlebars Templates

Handlebars is a simple web template system with minimal logic support. The
templates written in this language will be compiled and executed with specific
context to get the result. It's main purpose is for HTML output but can be used
for other text based formats, too.

An template may look like:

    {% raw %}
    <ul>
    {{#each users}}
        <li>{{firstname}} {{lastname}}</li>
    {{/each}}
    </ul>
    {% endraw %}

## Expression Syntax

The Handlebars Expressions like seen above are written like (a double stash before,
followed with the content to be evaluated, followed by a closing double stash):

    {% raw %}
    {{ content goes here }}
    {% endraw %}

Special HTML characters are escaped automatically, but you may prevent this
with the following syntax:

### Escaping

The content may be a variable to use, some control methods or special helper
functions to include.

    {% raw %}
    Triple Stash {{{ }}} For Non-escape HTML
    {% endraw %}

### Comments

This is how you add comments in a Handlebars template:

    {% raw %}
    {{! Whatever is inside this comment expression will not be outputted  }}
    {% endraw %}

Or if your comment may contain other handlebars tokens use:

    {% raw %}
    {{!-- this may contain {{tokens}} and more --}}
    {% endraw %}

Instead of the regular HTML comments they will not be outputted to the user.

### Blocks

Blocks in Handlebars are expression that has a block, an opening `{% raw %}{{# }}{% endraw %}`
followed by a closing `{% raw %}{{/ }}{% endraw %}`. See some more examples below.

    {% raw %}
    {{#each}}Content goes here.{{/each}}
    {% endraw %}

Here is an if block

    {% raw %}
    {{#if someValueIsTrue}}Content goes here{{/if}}
    {% endraw %}

### Helpers

An helper is an additional method working with some content or variable. They
may be used as simple functions or block expressions or both depending on it's
implementation.

Calls to a helper may also have literal values passed to them either as parameter
arguments or hash arguments. See the helper's description how it needs them.

Supported literals include numbers, strings, true, false, null and undefined.

Inline helpers are used as:

    {% raw %}
    {{helperName arg1 arg2 arg3 mapArg="val"}}
    {% endraw %}

Block helpers are used as:

    {% raw %}
    {{#helperName arg 1 arg2}}
    other stuff
    {{/helperName}}
    {% endraw %}

### Paths (with dot notation)

A path in Handlebars is a property lookup. If we have a name property that
contains an object, such as:

    {% raw %}
    objData = {name: {firstName: "Michael", lastName:"Jackson"}}
    {% endraw %}

We can use nested paths (dot notation) to lookup the property you want, like this:

    {% raw %}
    {{name.firstName}}
    {% endraw %}

### Parent Path ../

Handlebars also has a parent path ../ to lookup properties on parents of the
current context. Thus with a data object such as this:

    {% raw %}
    shoesData = {groupName:"Celebrities", users:[{name:{firstName:"Mike", lastName:"Alexander" }}, {name:{firstName:"John", lastName:"Waters" }} ]};
    {% endraw %}

We can use the parent path ../ to get the groupName property:

    {% raw %}
    ​<script id="shoe-template" type="x-handlebars-template">​
       {{#users}}​
        <li>{{name.firstName}} {{name.lastName}} is in the {{../groupName}} group.</li>​
        {{/users}}
    ​</script>
    {% endraw %}

## Built-In Helpers

### if block

A block in this condition will only be rendered if the given argument is empty
or null.

    {% raw %}
    {{#if author}}
      Author: {{firstName}} {{lastName}}
    {{/if}}
    {% endraw %}

It can also contain an alternative to use if not:

    {% raw %}
    {{#if author}}
      Author: {{firstName}} {{lastName}}
    {{else}}
      No Author defined.
    {{/if}}
    {% endraw %}

And finally you can also chain multiple conditional blocks together:

    {% raw %}
    {{#if isActive}}
      Is working now...
    {{else if isInactive}}
      Is inactive.
    {{else}}
      Unknown state.
    {{/if}}
    {% endraw %}

### unless block

You can use the unless helper as the inverse of the `if` helper. His content will
be rendered if the expression returns a falsy value.

    {% raw %}
    {{#unless author}}
      No Author defined.
    {{/if}}
    {% endraw %}

### each block

You can iterate over a **list** using this. Inside the block, you can use `this` to
reference the element being iterated over.

    {% raw %}
    {{#each people}}
    - {{this}}
    {{/each}}
    {% endraw %}

If it contains objects you may use their values:

    {% raw %}
    {{#each people}}
    - {{first}} {{this.last}}
    {{else}}
      No people assigned!
    {{/each}}
    {% endraw %}

As you see you may use the `this` value explicitly or directly use the object's
properties. And like in the `if` helper an additional `else` part will be rendered
if nothing to iterate is found.

When looping through items in each, you can optionally reference the current loop
index with '@index':

    {% raw %}
    {{#each people}}
    {{@index}}: {{first}} {{this.last}}
    {{else}}
      No people assigned!
    {{/each}}
    {% endraw %}

Also the first and last steps of iteration are noted via the '@first' and '@last'
variables when iterating over an array. When iterating over an object only the
'@first' is available.

Nested each blocks may access the iteration variables via depth based paths. To
access the parent index, for example, '{% raw %}{{@../index}}{% endraw %}' can be
used.

The each helper also supports block parameters, allowing for named references
anywhere in the block:

    {% raw %}
    {{#each array as |value key|}}
      {{#each child as |childValue childKey|}}
        {{key}} - {{childKey}}. {{childValue}}
      {{/each}}
    {{/each}}
    {% endraw %}

> It is not usable on objects, only on lists or lists of objects.

### with block

Normally, Handlebars templates are evaluated against the context passed into the
compiled method. But you may shift the current context for a section of a template
by using the built-in with block helper.

    {% raw %}
    {{#with author}}
      Author: {{firstName}} {{lastName}}
    {{/with}}
    {% endraw %}

`with` can also be used with block parameters to define known references in the
current block. The example above can be converted to

    {% raw %}
    {{#with author as |myAuthor|}}
      Author: {{myAuthor.firstName}} {{myAuthor.lastName}}
    {{/with}}
    {% endraw %}

This can also contain an `else` section which will display only when the passed
value is empty.

### lookup

The lookup helper allows for dynamic parameter resolution using Handlebars variables.
This is useful for resolving values for array indexes.

    {% raw %}
    {{#each bar}}
      {{lookup ../foo @index}}
    {{/each}}
    {% endraw %}

### raw block

Raw blocks are available for templates needing to handle unprocessed mustache blocks.

    {% raw %}
    {{{{raw-helper}}}}
      {{bar}}
    {{{{/raw-helper}}}}
    {% endraw %}

This keeps the internal handlebars syntax untouched.

## Alinex Helpers

If handlebars are used through the [validator](http://alinex.github.io/node-validator/README.md.html#handlebars)
a lot more and powerful helpers are available.


{!docs/abbreviations.txt!}

