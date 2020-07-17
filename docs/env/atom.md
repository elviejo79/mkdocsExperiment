# Atom

After using Sublime Text 3 for some time I was impressed about [Atom](https://atom.io/).
Atom is mostly based on JavaScript itself to be easy expendable with plugins. Because of
that there are a lot of extensions for nearly every language and need.
Now atom really grew to a full fledged IDE.

![Atom Editor](atom.png)

## Installation

That is an easy task, only download and install it directly from their homepage.

After that you may install the needed plugins, but only install what you really needs
else it may get bloated. To reset everything remove the directory `~/.atom`.

## Plugins

### General

- atom-ide-ui - tools for ide
- character-table - to easy find any UTF character

### Extended

- `project-manager` - helps to switch between different workspaces
- `highlight-selected` - show all occurrences of the selected text
- `markdown-pdf` - transform markdown into PDF
- `pdf-view` - view PDF files inline
- `open-in-browser` - to open HTML from the project tree
- `keybinding-cheatsheet` - display possible keys
- `tool-bar-atom` - customizable toolbar

### Programming

- `docblockr` - helper to write inline documentation
- `todo-show` - opens a pane for all to-do's
- `split-diff` - shows visual diff between split panes
- `atom-beautify` - highlight some included languages
- `pigments` - color display on definition
- `platformio-ide-terminal` - integrated console
- `atom-ternjs` - auto completion
- `linter-json-lint` - JSON linter

### TypeScript

- `ide-typescript` - language support
- `linter-tslint` - TypeScript linter

### ES.Next & Co

- `linter-eslint` - included linting
- `flow-ide` - to lint flow syntax and auto complete
- `autocomplete-modules` - for require/import auto completion

Also there are language and linter plugins for nearly every language and script
you use.

## Basic Shortcuts

At first it is very easy and a lot like sublime. The major shortcut to know for a
command search is: ++ctrl+shift+p++

Additionally the following may be helpful:

- ++ctrl+backslash++ will toggle the file tree view
- ++alt+backslash++ switch focus to/from tree view
- ++ctrl+t++ open fuzzy find for files in current project
- ++ctrl+b++ open fuzzy search to search in open files
- ++alt+shift+p++ switch projects (using project-manager)

## Editing

Move Cursor

- ++ctrl+left++ move to beginning of word
- ++ctrl+right++ move to end of word
- ++ctrl+g++ go to line (line:column)

Edit

- ++ctrl+up++ / ++ctrl+down++ moves the current line or selected lines up/down

Multi cursor

- ++alt+shift+up++ / ++alt+shift+down++ adds another cursor one line above/below
- ++ctrl+left-button++ add or remove another cursor
- ++ctrl+d++ / ++ctrl+u++ ad and remove cursor at the next match of selected word
- ++esc++ to remove multi cursor and go back to a single one

Maybe extend the key mapping with the following:

```cson
'.platform-linux atom-text-editor':
  'shift-alt-L': 'editor:split-selections-into-lines'
```

## Code Specific Helpers

Show Help

- ++ctrl+shift+m++ shows a rendered preview (realtime)
- ++alt+shift+t++ shows the to-do list (using `todo-show`)

## Using Git

Modify

- ++ctrl+shift+h++ show menu of all git commands
- ++ctrl+shift+a++ adds new files
- ++ctrl+shift+a++ ++s++ show status of files
- ++ctrl+shift+a++ ++c++ commit changes (give a message and save ++ctrl+s++)
- ++ctrl+shift+a++ ++p++ add all changed files and commit + push to remote


{!docs/abbreviations.txt!}

