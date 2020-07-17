# Modules

The modules will help you to start developing your projects. They can sometimes be also used as CLI tools in bash script. I try to make them as general as possible, so they met not only my first use case but also a lot others, too.

Have a look at them and find out what you need and what not. Include them as needed. They are all written with TypeScript definitions, so if you can benefit of the TypeChecking and tooling help.

![validator icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/11623256/json-schema-logo.png){: .right .icon}

## Validator

Like the [alinex-datastore](https://alinex.gitlab.io/node-datastore) it allows to load and read data structures from different locations and formats. But the validator adds checking and optimizing the data structure. This is done using predefined schema definitions which are powerful and rule based.
Using this module you may read configuration, describe structures, call REST services and be prevented against injection.

-   [Source](https://gitlab.com/alinex/node-validator)
-   [Documentation](https://alinex.gitlab.io/node-validator)

![datastore icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/11662964/exchange.png){: .right .icon}

## DataStore

The data store is a in memory store for small to medium sized data structures. It allows you to read, work and write in different formats and therefore also to transform between them.<br />
It can be used to display, access and modify specific data from various formats or transform between them. It supports protocols like file, sftp, http, https... with formats like JSON, BSON, XML, INI, properties and more also in compressed version.

-   [Source](https://gitlab.com/alinex/node-datastore)
-   [Documentation](https://alinex.gitlab.io/node-datastore)

![core icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/13521613/structure.png){: .right .icon}

## Data

This is a collection of methods to work on JavaScript data structures which are a combination of objects, array and other data types. These methods are: clone to make a exact copy of the complete data structure, filter to access and transform specific elements and merge to bring two or more structures together using different merge methods.

-   [Source](https://gitlab.com/alinex/node-data)
-   [Documentation](https://alinex.gitlab.io/node-data)

![core icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/13521754/flow-chart.png){: .right .icon}

## Async

Multiple methods to make working with promises easier: `delay`, `retry`, `each`, `map`, `filter` and `parallel`

-   [Source](https://gitlab.com/alinex/node-async)
-   [Documentation](https://alinex.gitlab.io/node-async)

![core icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/11665107/bw.png){: .right .icon}

## Core

The core module is a base for all of my alinex modules. It has some common methods which are necessary in nearly any of my packages.<br />
It includes error and exit handler and also displays the logo, mainly used for CLI.

-   [Source](https://gitlab.com/alinex/node-core)
-   [Documentation](https://alinex.gitlab.io/node-core)

![node-js icon](nodejs.png){: .right .icon}

## Older NodeJs Modules

-   [exec](http://alinex.github.io/node-exec/) async execution control for other processes
-   [database](http://alinex.github.io/node-database/) wrapper module supporting mysql and more
-   [fs](http://alinex.github.io/node-fs/) enhancement of the node filesystem library
-   [mail](http://alinex.github.io/node-mail/) easy mail sending
-   [media](http://alinex.github.io/node-media/) media file analyzation and manipulation
-   [server](http://alinex.github.io/node-server/) basic webserver based on express
-   [handlebars](http://alinex.github.io/node-handlebars/) helper collection for handlebars templates
-   [report](http://alinex.github.io/node-report/) easy report generation using markdown
-   [ssh](http://alinex.github.io/node-ssh/) ssh port tunneling made easy
-   [table](http://alinex.github.io/node-table/) working with table like data
-   [codedoc](http://alinex.github.io/node-codedoc/) a tool to extract documentation out of code

## Outdated

-   [Validator](https://github.com/alinex/node-validator/) replaced by new [Validator](https://alinex.hitlab.io/node-validator)
-   [Core](https://github.com/alinex/node-core/) replaced by new [Core](https://alinex.hitlab.io/node-core)
-   [util](http://alinex.github.io/node-util/) major parts replaced by [Data](https://alinex.hitlab.io/node-data)
-   [builder](http://alinex.github.io/node-builder/) no longer necessary
-   [format](http://alinex.github.io/node-format/) replaced by [DataStore](https://alinex.hitlab.io/node-datastore)

{!docs/abbreviations.txt!}
