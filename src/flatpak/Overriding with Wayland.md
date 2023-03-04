# Wayland

## Overriding electron apps with Wayland

This was tested using Fedora 37. The example below is using Logseq.

```sh
flatpak override --user --socket=wayland --env=GDK_BACKEND=wayland com.logseq.Logseq
```

Then try to open with:

```sh
GDK_BACKEND=wayland flatpak run com.logseq.Logseq --enable-features=UseOzonePlatform --ozone-platform=wayland
```

Now you can edit with `Alacarte`/`Menu Editor`:

Instead of

```sh
/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=run.sh --file-forwarding com.logseq.Logseq @@u %U @@
```

Use this

```sh
/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=run.sh --file-forwarding com.logseq.Logseq --enable-features=UseOzonePlatform --ozone-platform=wayland @@u %U @@
```

## Obsidian

Obsidian has a dedicated environment variable:

```sh
flatpak override --user --env=OBSIDIAN_USE_WAYLAND=1 md.obsidian.Obsidian
```
