# Python VirtualEnvs

## VirtualEnvs on Fish Shell

First you need to use [oh my fish](https://github.com/oh-my-fish/oh-my-fish), preferably with my [.dotfiles](../arch/initial_setup.md#Using my .dotfiles with TMUX and Fish Shell)

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