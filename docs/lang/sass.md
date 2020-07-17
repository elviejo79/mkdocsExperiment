title: Sass

# Sass CSS Preprocessor

!!! info

    Since Version 3 SCSS is the official Syntax, which adds braces to the definition. There,fore only this format is described here.

## Variables

To reuse the same information again, variables may be used. They need a pound sign in front.

```scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

## Nesting

With nesting the CSS will become more organized:

```scss
div {
  // will become: div a { .... }
  a {
    color: red;
    // The parent selector helps to further extend the same element.
    // div a:hover { .... }
    &:hover {
        font-weight: bold;
    }
  }
}

.test {
  // It can also be used to style the outer selector in a certain context, such
  // as a body set to use a right-to-left language.
  // [dir=rtl] .test { ... }
  [dir=rtl] & {
    margin-left: 0;
    margin-right: 10px;
  }

  // You can even use it as an argument to pseudo-class selectors.
  // :not(.test) { ... }
  :not(&) {
    opacity: 0.8;
  }

  // Also a suffix to the existing class can be set.
  // .test--dark { ... }
  &--dark {
    color: white;
}
```

## Mixins

```scss
@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}
.box { @include transform(rotate(30deg)); }
```

## Extend

You define a block of scripts as Extend block, which may be used anywhere:

```scss
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}
```

## Operators

Mathematical operators are possible, too.

```scss
article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}
```

## At-rules

- `@use`, `@forward`, `@import` loads mixins, functions, and variables.
- `@mixin` and @include makes it easy to re-use chunks of styles.
- `@function` defines custom functions that can be used in SassScript expressions.
- `@extend` allows selectors to inherit styles from one another.
- `@at-root` puts styles within it at the root of the CSS document.
- Flow control rules like `@if`, `@each`, `@for` and `@while` control whether or how many times styles are emitted.

## Builtin Modules

- `sass:math` module provides functions that operate on numbers.
- `sass:string` module makes it easy to combine, search, or split apart strings.
- `sass:color` module generates new colors based on existing ones, making it easy to build color themes.
- `sass:list` module lets you access and modify values in lists.
- `sass:map` module makes it possible to look up the value associated with a key in a map, and much more.
- `sass:selector` module provides access to Sass’s powerful selector engine.
- `sass:meta` module exposes the details of Sass’s inner workings.

More details can be found in [Sass Documentation](https://sass-lang.com/documentation).

{!docs/abbreviations.txt!}
