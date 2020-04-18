# GPG, SSH Auth with Keybase

Installing requirements:

    sudo pacman -S kbfs keybase keybase-gui micro

If you are having problems with gpg-agent as ssh agent mode, restart gnupg from scratch (export your important keys first)

    rm -rf ~/.gnupg

Login to Keybase using the GUI or `keybase login`. Now import private key and public keys from keybase:

    keybase pgp export --secret | gpg --allow-secret-key-import --import
    keybase pgp export | gpg --import

Setup GPG Agent:

    micro ~/.gnupg/gpg-agent.conf

With the below contents:

    enable-ssh-support

    default-cache-ttl-ssh 10800
    max-cache-ttl-ssh 10800

    pinentry-program /usr/bin/pinentry-gnome3

Setup PAM environment as extra:

    micro ~/.pam-environment

With the below contents:

    SSH_AGENT_PID	DEFAULT=
    SSH_AUTH_SOCK	DEFAULT="${XDG_RUNTIME_DIR}/gnupg/S.gpg-agent.ssh"

Add your keys to SSH control file, that keeps the "authorized" keys to use by the emulated SSH agent:

    ls ~/.gnupg/private-keys-v1.d/ | sed s/.key// >> ~/.gnupg/sshcontrol

Now logout and login again, or event better, reboot your machine. Check your SSH keys with:

    ssh-add -l

If you are getting errors like error fetching identities: agent refused operation then you need to remove offending keys that are not **A**uthorization keys.
You can check them with:

    gpg --list-keys --with-keygrip

Then comment out the keys with `#` or remove auth with `!` at  `~/.gnupg/sshcontrol`:

    micro ~/.gnupg/sshcontrol