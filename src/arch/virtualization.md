<!-- toc -->

# Virtualization in Archlinux - Docker, Kubernetes, KVM

Tips and tricks for a ideal initial setup

<!-- toc -->

## Docker Setup

    yay -S docker nvidia-container-toolkit
    sudo usermod -a -G docker $(whoami)
    sudo systemctl start docker
    docker info

## Kubernetes using K3s (K8s on K3s)

First you need to setup docker like above.

    yay -S helm kubectl k3s-bin
    set -gx KUBECONFIG /etc/rancher/k3s/k3s.yaml  #for fish shell
    export KUBECONFIG=/etc/rancher/k3s/k3s.yaml   #for bash shell
    sudo systemctl start k3s
    sudo chown -R root.docker /etc/rancher/k3s
    kubectl get pods --all-namespaces
    helm ls --all-namespaces

### Kubernetes Dashboard using fish shell

    set GITHUB_URL https://github.com/kubernetes/dashboard/releases
    set VERSION_KUBE_DASHBOARD (curl -w '%{url_effective}' -I -L -s -S ${GITHUB_URL}/latest -o /dev/null | sed -e 's|.*/||')
    k3s kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml

#### Deleting the dashboard

    k3s kubectl delete ns kubernetes-dashboard


## KVM Setup on Arch

### Basic installation

    sudo pacman -S --needed qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat ebtables iptables
    sudo yay -S --needed libguestfs
    sudo systemctl enable --now libvirtd.service

### Enable normal user account to use KVM

    sudo micro /etc/libvirt/libvirtd.conf

Uncomment these lines:

    unix_sock_group = "libvirt"
    unix_sock_rw_perms = "0770"

Back to shell:

    sudo usermod -a -G libvirt $(whoami)
    newgrp libvirt
    sudo systemctl restart libvirtd.service

In my test we need more configuration to not get a laggy 2D experience, even on Manjaro KDE.
