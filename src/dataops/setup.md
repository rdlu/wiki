# Other Data Tools

## More languages to play on Manjaro and a notebook

    yay -S nteract-bin ihaskell-git r sagemath-jupyter julia

## NVIDIA Accelerated Docker containers on ArchLinux

    yay -S nvidia-container-runtime
    sudo tee /etc/docker/daemon.json <<EOF
    {
        "default-runtime": "nvidia",
        "runtimes": {
            "nvidia": {
                "path": "/usr/bin/nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }
    EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    docker run nvidia/cuda:10.1-base nvidia-smi

## Compiling Psycopg2 for Lambda on ArchLinux

Please pay attention to the version numbers, change to newer versions as appropriate.

    cd ~/Downloads
    pacman -S postgresql postgresql-libs
    wget http://initd.org/psycopg/tarballs/PSYCOPG-2-7/psycopg2-2.7.5.tar.gz
    tar xf psycopg2-2.7.5.tar.gz
    cd psycopg2-2.7.5/
    sed -i 's/libpq.a/libpq.so/g' setup.py
    gedit setup.cfg

Change setup.cfg build_ext section to that:

    [build_ext]
    define = 
    pg_config = /usr/bin/pg_config
    use_pydatetime = 1
    mx_include_dir = 
    have_ssl = 0
    static_libpq = 1
    libraries = ssl crypto

After that, we will finally build:

    python setup.py build
    cd build/lib.linux-x86_64-3.7
    cp -R psycopg2/ ~/Projects/{MY_LAMBDA_PROJECT}/

## Exporting PDF using `jupyter nbconvert`

    yay -S texlive-most --needed