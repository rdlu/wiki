# KVM Setup on Arch

## Basic installation

    sudo pacman -S --needed qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat ebtables iptables
    sudo yay -S --needed libguestfs
    sudo systemctl enable --now libvirtd.service

## Enable normal user account to use KVM

    sudo micro /etc/libvirt/libvirtd.conf

Uncomment these lines:

    unix_sock_group = "libvirt"
    unix_sock_rw_perms = "0770"

Back to shell:

    sudo usermod -a -G libvirt $(whoami)
    newgrp libvirt
    sudo systemctl restart libvirtd.service

In my test we need more configuration to not get a laggy 2D experience, even on Manjaro KDE.
