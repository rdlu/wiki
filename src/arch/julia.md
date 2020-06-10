# Julia

## nteract notebooks plus IJulia Packages managed by pacman

It's a wonderful idea to delegate the package updates to the operating system.

ArchLinux AUR has many Julia packages already, including `julia-ijulia` for notebooks

```sh
yay -S julia-distrohelper julia-loadpath
```

**Reboot** or logout/login from your user, because these 2 packages manages the julia's loadpath using system packs as a fallback. Without that Julia envs will not recognize the system installed packages.

```sh
yay -S --needed julia-ijulia nteract-bin
```

Now you can install any packages starting with `julia-*` and notebook apps like `nteract-bin` will recognize Julia as a option to notebook environments.
