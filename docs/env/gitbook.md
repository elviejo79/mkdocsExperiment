# GitBook

You can create static pages from [Markdown](../lang/markdown.md) with static site generators like [GitBook](https://www.gitbook.com/).

GitBook is also an online platform for writing and hosting documentation using open
source book format and tool-chain. It has a big user base and can be integrated
with GitHub.

It is open source and you can also host it yourself or add it to your GitLab pages repository like I did with this book.

## Setup

### Local setup

Then I set up the repository by placing a `book.json` in it's root directory. It
should at least contain the definition of the root directory used for the book.
I need this because I store my documentation beside the code in its own directory.

```javascript
{
  "root": "./doc"
}
```

Then you need the `doc` directory with the following files:

-   `doc/README.md`
-   `doc/SUMMARY.de`

The `SUMMARY.de` should contain your table of contents as bullet list with
optional headings for chapters. This could look like:

```markdown
# Summary

-   [Introduction](README.md)
-   [Alinex Project](alinex.md)

### Standards

-   [Quality](quality.md)
-   [Documentation](doc.md)
-   [File Structure](filestructure.md)
-   [Exit Codes](exitcodes.md)

### Modules

-   [Alinex](modules.md)
-   [3rd Party](3rd-party.md)
```

### GitBook Cloud

Firstly I had to install the GitHub Integrations Plugin within the GitBook options.
Also the GitHub had to be allowed to use for GitBook.

To have a special cover on the PDF, ePub version of the book is done by providing two images:

-   `cover.jpg`
-   `cover_small.jpg`

A good cover should respect the following guidelines:

-   Size of 1800x2360 pixels for `cover.jpg`
-   Size of 200x262 pixels for `cover_small.jpg`
-   No border
-   Clearly visible book title
-   Any important text should be visible in the small version

And to set the layout you may use:

```json
{
    "pdf": {
        "pageNumbers": true,
        "headerTemplate": " ",
        "footerTemplate": " "
    }
}
```

### GitLab

First you have to setup the CI Pipeline to create and upload your pages. For GitBook this is done by placing the following `.gitlab-ci.yml` file in your projects root:

```yml
# requiring the environment of NodeJS 8.9.x LTS (carbon)
image: node:8.9

# add 'node_modules' to cache for speeding up builds
cache:
    paths:
        - node_modules/ # Node modules and dependencies

before_script:
    - npm install gitbook-cli -g # install gitbook
    - gitbook fetch latest # fetch latest stable version
    - gitbook install # add any requested plugins in book.json
    #- gitbook fetch pre # fetch latest pre-release version

# the 'pages' job will deploy and build your site to the 'public' path
pages:
    stage: deploy
    script:
        - gitbook build . public # build to public path
        - find public -type f -iregex '.*\.\(htm\|html\|txt\|text\|js\|css\)$' -execdir gzip -f --keep {} \; # make compressed files
    artifacts:
        paths:
            - public
        expire_in: 1 week
    only:
        - master # this job will affect only the 'master' branch
```

Push this to your master to add the pipelines.

Now you should enable pages under `Settings > Pages`. So whenever you push new changes to your master the job will run and recreate your static pages.

## Writing Documentation

All the pages are written as single files in the `doc` folder using
[Markdown](https://toolchain.gitbook.com/syntax/markdown.html) language. It is nearly
the same as used on GitHub.

**Attention:** In contrast to the other markdown parsers there should be no space between
the code tag and the language.

### Plugins

#### Layout

To improve the layout of the book I use three different plugins:

```json
{
    "plugins": ["toggle-chapters", "navigator", "downloadpdf"]
}
```

This will open/close chapters like folders, display a page navigation on the
right side and a link to download as PDF on the top line.

To optimize the navigator output set the following in `styles/ebook.css` and
`styles/pdf.css` to disable navigator:

```css
#anchors-navbar,
#goTop {
    display: none;
}
```

And for `styles/website.css` add this to optimize display and remove for small
display:

```css
#anchors-navbar {
    color: darkgray;
    right: 28px;
    top: 45px;
}
#goTop {
    display: none;
}
@media (max-width: 660px) {
    #anchors-navbar {
        display: none;
    }
}
```

#### ToDo

As already used in GitHub markdown this plugin allows to write ToDo lists with
check boxes which may be checked:

```json
{
    "plugins": ["todo"]
}
```

Now you may create a checklist using:

-   [ ] Mercury
-   [x] Venus
-   [x] Earth (Orbit/Moon)
-   [x] Mars
-   [ ] Jupiter
-   [ ] Saturn
-   [ ] Uranus
-   [ ] Neptune
-   [ ] Comet Haley

This is done using:

    - [ ] Mercury
    - [x] Venus
    - [x] Earth (Orbit/Moon)
    - [x] Mars
    - [ ] Jupiter
    - [ ] Saturn
    - [ ] Uranus
    - [ ] Neptune
    - [ ] Comet Haley

#### Mermaid Graphs

Using [Mermaid](https://knsv.github.io/mermaid/) it is possible to include easy
flowcharts without a specific program. It is written as plaintext and converted into
chart on display.

To make this work the following plugin have to be defined:

```json
{
    "plugins": ["mermaid-gb3"]
}
```

As an example can be:

    ```mermaid
    graph TD;
      A-->B;
      A-->C;
      B-->D;
      C-->D;
    ```

#### PlantUML

[PlantUML](http://plantuml.com/) is another format to make graphs out of text descriptions like mermaid.

The plugin is loaded using:

```json
{
    "plugins": ["puml"]
}
```

And the diagram may be added as:

    {% plantuml %}
    Bob->Alice : hello
    {% endplantuml %}

### Final Setup

#### book.json

```json
{

  "root": "./doc",

  # only gor direct gitbook.com cloud hosting with enabled pdf
  "pdf": {
    "pageNumbers": true,
    "headerTemplate": " ",
    "footerTemplate": " "
  },

  "plugins": [
    "todo", "mermaid-gb3", "puml",
    "navigator",
    "collapsible-chapters", "hide-published-with", "insert-logo"
  ],
  "pluginsConfig": {
    "insert-logo": {
        "url": "https://alinex.gitlab.io/logo.png",
        "style": "background: none;"
    },

    # also only for pdf on github.com
    "downloadpdf": {
      "base": "https://www.gitbook.com/download/pdf/book/alinex/nodejs",
      "label": "Download PDF",
      "multilingual": false
    }
  }
}
```

#### Further files

```coffee
doc/README.md           # Introduction
doc/SUMMARY.md          # Page index of book
doc/cover.jpg           # eBook cover
doc/cover_small.jpg     # small cover
doc/styles/ebook.css    # user style
doc/styles/pdf.css      # user style
doc/styles/website.css  # user style
```


{!docs/abbreviations.txt!}

