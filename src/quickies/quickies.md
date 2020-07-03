# Git    

## Redundancy

### Add Multiple repositories

Refer to my dotfiles/fish/functions.fish file

### Push to all remotes at same time

Creating an alias is a good idea

    git config --global alias.pushall '!git remote | xargs -L1 git push --all'