# Podman

## Running a container with Wireguard && Current User

```fish
set -lx UID (id -u)
set -lx GID (id -g)

podman run -d --user 0:0 --userns keep-id:uid=$UID,gid=$GID \
    --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
    --privileged=true \
```

