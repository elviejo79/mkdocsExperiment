title: DevOps

# DevOps Tools

DevOps is a term for a group of concepts that, while not all new, have catalyzed into a movement and are rapidly spreading throughout the technical community. It is also characterized by operations staff making use many of the same techniques as developers for their systems work.

The tools listed here are mostly for developers, which want to help automate operation tasks. That means you can't directly use them, you have to use them as a base and need to setup and specialize them for your individual needs. But they help you to do this.

![bash-lib icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/10069235/images__1_.jpeg){: .right .icon}

## Bash Library

This library can be used to easily build powerful bash scripts. It's use is very simple and can be integrated fast. I for myself use it within the `ssh-control` an application to manage hundreds of virtual machines in a standardized way and my `admin-utils` collection which controls server processes, backups and also for data conversion. This two tools are closed source because they are specific to my work environment. But you can make such things using the bash-lib easily yourself.

-   [Source](https://gitlab.com/alinex/bash-lib)
-   [Documentation](https://alinex.gitlab.io/bash-lib)

![datastore icon](https://assets.gitlab-static.net/uploads/-/system/project/avatar/11662964/exchange.png){: .right .icon}

## DataStore

The data store is a in memory store for small to medium sized data structures. It allows you to read, work and write in different formats and therefore also to transform between them.<br />
It can be used to display and extract specific data from various formats or transform between them. It supports protocols like file, sftp, http, https... with formats like JSON, BSON, XML, INI, properties and more also in compressed version.

-   [Source](https://gitlab.com/alinex/node-datastore)
-   [Documentation](https://alinex.gitlab.io/node-datastore)

![node-js icon](nodejs.png){: .right .icon}

## Older NodeJS Helpers

-   [monitor](http://alinex.github.io/node-monitor/) server application for monitoring IT systems
-   [scripter](http://alinex.github.io/node-scripter/) environment to make powerful scripts

{!docs/abbreviations.txt!}
