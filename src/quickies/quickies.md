# Git    

## Redundancy

### Add Multiple repositories

Refer to my dotfiles/fish/functions.fish file

### Push to all remotes at same time

Creating an alias is a good idea

    git config --global alias.pushall '!git remote | xargs -L1 git push --all'

# Keychron with VIA inside Linux

It is supported only through Chromium

Add to `/etc/udev/rules.d/50-keychron-chrome.rules`

```
KERNEL=="hidraw*", ATTRS{idVendor}=="3434", MODE="0664", GROUP="plugdev"
```

Reboot your system to be sure, then go to https://usevia.app

Fedora does not have a plugdev group like ArchLinux, so change the group to something your user belongs.