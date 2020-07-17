title: Introduction

# Alinex Development Guide

The book works as a ground documentation containing information and help in the area of web
development. This includes everything necessary from the environment, server and client
programming including a lot of helpful tools and technologies. Also if not really web components but two systems interchanging information through a web interface which is often used today this document will help.

It is a big area with a lot of different competing fast and always progressing technologies.
That makes it difficult to keep up to date with everything. Therefore this book can't be complete
or up to date at any time. But it will evolve and you may look again some time.

This is a book explaining all the major parts and development background around
the Alinex named coding projects. But it is not completely specific to Alinex and more a book
to learn and use the different IT technologies with best practice from the Alinex modules.
The epics included are the ones I decided to look at and if something is missing maybe I
had not seen this or decided to not try it out till now. Some descriptions are
made while learning so this is also a reference for myself. So it is also a knowledge base and collection of my personal progress in learning the different technologies and often contains short overviews and only the essential facts of the original documentation.

The decisions from this book are not real hard facts but a definition for me to follow as
possible. It may change as the world around changes. And you don't have to
follow this standards but it helps for all which are working deeply with the Alinex
modules and participating to it to understand the parts behind.
Some of the Alinex modules use an older standard (like the CoffeeScript based ones) but will
eventually be upgraded to newer versions.

> Keep in mind that the book explains a lot of technologies around web
> development but it is neither meant as a complete guide to teach the technologies more like
> a best practice and how to bring it all together.

## Structure of Book

The top level chapters are:

1. **Home**

    With this introduction about the book, the Alinex Namespace and myself. Including a short blog about what is going on and on what I work.

2. **[Environment](env/README.md)**

    All the tools around, which you may need to develop and work on the code. It describes programs and technologies often used to do the development work. And it contains all which is needed within the whole development cycle including CI/CD.

3. **[Technologies](lang/README.md)**

    Programming languages, tools and technologies which are used in the client server area. Only the ones with a big appearance here are moved out to own chapters below.

4. **[Solutions](solutions/README.md)**

    This chapter will show ready to use solutions for programming or as application. This may contain tools from the Alinex namespace or others.

5. **[JavaScript](js/README.md)**

    Programming in JavaScript and languages which translates into JavaScript like CoffeeScript, TypeScript and Flow. Usage may be on client but also on the server based on Node.JS.

6. **[Rust](rust/README.md)**

    As a system programming language which features stability Rust may be used on the server side in the client server programming.

## Accessibility

The book is made responsive meaning it's rendering will vary and always be optimizes for the device and window screen you use. You may access it with the smartphone as good as within your PC Browser.

Specially on your smartphone you can not only access it in your browser but also make it look more like an app by calling "Add to Home Screen" from the browser menu. This technology comes from the PWA but as the book is static no service-worker or active elements are included.

## Requirements

As far as possible Alinex tries to not be restricted by a specific system or service
but to come to an end it focuses on specific systems and services.

At first this documentation is based on Debian Linux examples but it
will work on any other Linux system or mac with some minor changes. You may also
work with windows by replacing the few OS specific calls with appropriate ones.

All the environment tools used here may also be replaced by other ones for their
work.

And at last the most important thing the end results are mostly not OS specific and
will run on any system.

## Development basics

My key concepts are "**Configuration over Implementation**" and "**Keep it Simple**".

All the different modules in the Alinex namespace are loose coupled but optimal
integrated. At first they solve my own problems but I will enlarge their functionality
and often my problems are the problems of others, too.

I believe in open source and all the tools which are mentioned here are free for
open source projects. But you mostly can also use them for private use.

## Conclusion

This is not only written to teach others but as said before mainly to keep
the knowledge as a reference for myself. Also it is not statically written but a
type of living book which evolve and grow in time hopefully as fast as technology
goes on. So read it once, but come back again to see the newest changes.

> This book is also available as PDF or ePub, so if you need a static copy (not always fully up to date) download: [alinex-book.pdf](alinex-book.pdf) or [alinex-book.epub](alinex-book.epub).

{!docs/abbreviations.txt!}
