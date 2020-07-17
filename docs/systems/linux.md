# Linux

This is a small and incomplete collection of some Problems I had to fix, to keep them if I may need them again.

## Touch click events not working after suspend

Create a script which is called on resume event under `/etc/pm/sleep.d/0000trackpad`:

```bash
#!/bin/sh
case "$1" in
    resume)
        DISPLAY=:0.0 su <USER> -c '/usr/bin/synclient TouchpadOff=0' ;;
esac
```

## Tunnel Git through socks proxy

```bash
# open tunnel
ssh -D 1337 -q -C -N alinex@peacock.uberspace.de # Ctrl-C to stop
# use tunnel for git
git config --global http.proxy 'socks5://127.0.0.1:1337'
# remove tunnel from git
git config --global --unset http.proxy
```

{!docs/abbreviations.txt!}
