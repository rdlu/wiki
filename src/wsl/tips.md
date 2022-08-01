# WSL

## Backup using WSL2 + Compression

Inside a Power Shell terminal:

    wsl --export Distrod  - | zstd -10 -o "D:\archlinux-$(Get-Date -Format FileDateUniversal).tar"

You'll need `zstd` installed. Use scoop.sh inside PowerShell.

    scoop install zstd
