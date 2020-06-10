<!-- toc -->

# ArchLinux | Manjaro Based Setup

Tips and tricks for a ideal initial setup

<!-- toc -->

## Ranking mirrors for faster downloads

### Manjaro

     sudo pacman-mirrors -c Brazil,United_States

### ArchLinux

    curl -s "https://www.archlinux.org/mirrorlist/?country=BR&use_mirror_status=on" | sed -e 's/^#Server/Server/' -e '/^#/d' | rankmirrors -n 5 -

## Update system

   sudo pacman -Suy
   sudo pacman -S yay # only works on Manjaro

## Interesting terminal companions and programming languages

    yay -S micro-manjaro
    yay -S --needed thefuck nodenv nodenv-node-build-git rbenv ruby-build elixir fish tmux autoconf automake bison bind-tools fasd htop make patch ed fzf gcc mosh ruby tk yarn php php-fpm python-pip python-pillow python-numpy nfs-utils lsof strace tldr mosh
    tldr --update

## Interesting graphical interface apps and tools

    yay -S --needed adobe-source-sans-pro-fonts chromium deluge firefox-developer-edition otf-fira-code otf-fira-sans p7zip ttf-roboto ttf-ubuntu-font-family
    yay -S --needed albert-lite atom-editor-bin visual-studio-code-bin ttf-iosevka pinta simple-scan xsel python-pyusb

## [Interesting python packages](../python/initial_setup.md)

## Interesting utilities

    yay -S --needed telegram-desktop foliate flameshot neofetch

## No password for sudo

    sudo visudo -f /etc/sudoers.d/99-mine

Be careful with %wheel, I prefer to put just myself :D

    %wheel ALL=(ALL) NOPASSWD: ALL
    rodrigo ALL=(ALL) NOPASSWD: ALL

## Using my .dotfiles with TMUX and Fish Shell

    ```sh
    cd
    git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
    git clone https://gitlab.com/rdlu/dotfiles.git .dotfiles
    ln -s ~/.dotfiles/tmux.conf .tmux.conf
    tmux source ~/.tmux.conf
    ln -s ~/.dotfiles/fish/config.fish ~/.config/fish/config.fish
    ln -s ~/.dotfiles/fish/omf ~/.config/omf
    curl -L https://get.oh-my.fish | fish
    sudo chsh (whoami) -s /usr/bin/fish
    ```

After that restart your fish prompt and:

    omf update

`CTRL+A`; `SHIFT+U` for tmux plugins
