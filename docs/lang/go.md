# Go

_My first experience with go!_

In my effort to learn a new language I started to checkout `Go` and `Rust` both for
some days and do a bit in both to decide which one to choose for a deeper experience.
I worked a lot with Java later with NodeJS but both have some problems
for me. Java is not after my liking, while NodeJS is great for fast prototyping but
get problems if the project grows really big.

So in this article I describe the findings of `Go` as an notebook for myself.

## Installation

This is done really easy under Linux:

```shell
# download
curl -O https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz
# install
sudo tar -C /usr/local -xzf go1.8.1.linux-amd64.tar.gz
rm go1.8.1.linux-amd64.tar.gz
# set path to go and compiled programs
echo "export GOPATH=$HOME/go" >> ~/.bashrc
echo "export GOPATH=$HOME/go" >> ~/.profile
echo "export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin" >> ~/.bashrc
echo "export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin" >> ~/.profile
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
# install additional helpers
go get -u github.com/nsf/gocode
go get -u github.com/golang/lint/golint
```

Other installation methods also exist see the [Project Site](https://golang.org/doc/install).

You will find all the go related stuff below a `go` folder in your home directory.

## Learning

To dive into I worked through the following resources:

- [Getting Started](https://golang.org/doc/install)
- [Tour](https://tour.golang.org/welcome/1)
- [HowTo](https://golang.org/doc/code.html)
- [Effective Go](https://golang.org/doc/effective_go.html)

## File Structure

The go workspace (`~/go` under Linux) contains the following directories:

    src/                            # sources
      github.com/alinex/go-learn    # package source
        .git/                       # version control system
        ...
    pkg/                            # downloaded packages
    bin/                            # executable commands

## Editor

At first I use the `Atom` editor with the following plugins:

- `go-plus`
- `go-debug`
- `go-signature-statusbar`
- `formatter-gofmt`

No need for a style guide here because formatting is done by`gofmt`
on it's own on saving.

The above installers `go-signature-statusbar` will you show a reference using
`Alt-D` with the cursor on a specific symbol.

## Build Tools

All the possible tools are combined in the go cli tool:

    go build          # check if it can be compiled
    go install        # to build and make a runnable command
    go test           # run unit tests
    <name>            # to directly run it, because you have it in your path
    go get <path>     # fetch and install

## Documenting

To show your documentation in the browser start the `godoc` server:

    godoc -http=:6060 &                 # to start it in the background
    xdg-open http://localhost:6060/pkg/ # to open the package list in the browser

And then go in your browser to
[Learning Go](http://localhost:6060/pkg/github.com/alinex/go-learn/).


{!docs/abbreviations.txt!}

