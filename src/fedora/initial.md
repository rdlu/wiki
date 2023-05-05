# Fedora Setup

<!-- toc -->

## Yubikey with GPG

File `~/.gnupg/scdaemon.conf`:

```
pcsc-shared
pcsc-driver /usr/lib64/libpcsclite.so
card-timeout 5
disable-ccid
```


File `~/.gnupg/gpg-agent.conf`:

```
enable-ssh-support

default-cache-ttl-ssh 10800
max-cache-ttl-ssh 10800
max-cache-ttl 300
default-cache-ttl 300

pinentry-program /usr/bin/pinentry-gnome3
# pinentry-program /usr/bin/pinentry-curses
```