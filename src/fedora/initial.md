# Fedora Setup

<!-- toc -->

## Yubikey with GPG

This can help with Yubikey card not available after sleep.

Remove opensc

`sudo dnf remove opensc`

File `~/.gnupg/scdaemon.conf`:

```
reader-port Yubico Yubi
pcsc-shared

```

File `/etc/polkit-1/rules.d/10-pcsc-custom.rules`:

```
polkit.addRule(function(action, subject) {
    if (action.id == "org.debian.pcsc-lite.access_pcsc" &&
        subject.user == "rdlu") {
            return polkit.Result.YES;
    }
});

polkit.addRule(function(action, subject) {
    if (action.id == "org.debian.pcsc-lite.access_card" &&
        action.lookup("reader") == 'Yubico YubiKey OTP+FIDO+CCID 00 00' &&
        subject.user == "rdlu") {
            return polkit.Result.YES;
    }
});

```