# Python VirtualEnvs

## VirtualEnvs on Fish Shell

First you need to use [oh my fish](https://github.com/oh-my-fish/oh-my-fish), preferably with my [.dotfiles](../arch/initial_setup.md#using-my-dotfiles-with-tmux-and-fish-shell)

    omf install virtualfish
    pip install virtualfish
    vf install compat_aliases projects environment auto_activation global_requirements update_python 

Then for existing projects:

    vf new myprojectenv
    vf connect
    cd
    vf disconnect
    cd ~/Projects/myproject

Check if the environment auto changes.

## Pipenv

    pacman -S python-pipenv pyenv
    omf install pyenv
    pyenv install 3.7.7
    pipenv

Then on any project folder:

    pipenv install --python 3.7
