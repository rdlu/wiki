# Vivaldi Browser

## Installation

    yay -S --needed vivaldi

## Proprietary codecs

You can install from AUR also, but they are slow to download and build (downloading an entire Google Chrome deb, etc)

    yay -S --needed vivaldi-ffmpeg-codecs vivaldi-widevine vivaldi-codecs-ffmpeg-extra-bin

Or, **below** you can use the prebuilt binaries from Vivaldi to install in the user space.

## Vivaldi Browser Proprietary Media on user space (no root)

The `update-ffmpeg` and `update-widevine` scripts included in the Vivaldi install directory are provided to fix situations where proprietary media (AVC/H.264 and AAC) and Widevine (DRM/EME) respectively, are not setup correctly.

These scripts are primarily intended to be run as root (or under `sudo`) as they create and update files and directories that are root owned. However both support a command line option (`--user`) that adjusts their installation directories and thus allows them to be run without escalation.

The `--user` options were made for internal usage, with locally ‘unpacked’ copies of Vivaldi (i.e. not installed). However, it is possible to use them with standard installs (albeit with a little tweaking in the case of `update-widevine`).

## update-ffmpeg

To install proprietary media for just your current user issue the following:

```
/opt/vivaldi/update-ffmpeg --user
```

## update-widevine

To install Widevine for just your current user, a couple more steps are required. This is because the script still attempts to adjust symlinks within the (typically root owned) Vivaldi install directory.

You can adjust the script as it runs, to disable the symlink creation, which would otherwise cause failure:

```
sed -r 's/^ *(rm|ln) -f.*/:/' /opt/vivaldi/update-widevine | sh -eus -- --user
```

Now create meta data in your Vivaldi user data directory that points to alternative Widevine install location–_triple click to select the entire line_:

```
echo "{\"Path\":\"$HOME/.local/lib/vivaldi/WidevineCdm\"}" > "${XDG_CONFIG_HOME:-$HOME/.config}/vivaldi/WidevineCdm/latest-component-updated-widevine-cdm"
```

_**Note:** Change all “vivaldi” references in the above commands to “vivaldi-snapshot”, if you use the snapshot version._

(Original)[https://gist.github.com/ruario/995728dd8123540d331052e0ce648439]