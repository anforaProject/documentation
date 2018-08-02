# Installation guide - WIP

## 0. Prerequisites

We will using: 

- A Ubuntu based distro
- Root access on the machine
- A domain
- Python 3.6
- nginx (you can use apache too)

## 1. Setting up the environement

Update your package list and system:

    sudo apt update
    sudo apt upgrade

Older dristribution of ubuntu don't include python3.6 and we'll need it. 
Depending on your OS version you may need to install it.

Install the dev packages and necessary things for python to compile packages.

    sudo apt install build-essential libssl-dev libffi-dev python3-dev

You'll need git and yarn to download and install dependencies.

    sudo apt install git

To install yarn please read the instructions at their [official webpage](https://yarnpkg.com/lang/en/docs/install/#debian-stable)

### Other dependencies

* `redis-*` - Used to queue tasks and as in-memory data store structure
* `imagemagick` - Image related operations
* `ffmpeg` - Conversion of videos to mp4
* `nodejs` - Used for the streaming API

To install them use:

    sudo apt install imagemagick ffmpeg nginx redis-server redis-tools postgresql postgresql-contrib python3-pip

Finally we'll need `pipenv` a python utility to manage installed packages. To install `pipenv` use `pip`:

    sudo pip3 install --user pipenv

If you don't have pipenv command use

    sudo -H pip install -U pipenv

If you get the following error:

    Traceback (most recent call last):
      File "/usr/bin/pip3", line 9, in <module>
        from pip import main
    ImportError: cannot import name 'main'

It's a [known error](https://github.com/pypa/pip/issues/5447)

    sudo python3 -m pip uninstall pip && sudo apt install python3-pip --reinstall


## Set-up the project

### Installing the core

First you need to clone the project. The `stable` branch is master as others branchs contains changes that may break the instance.

    git clone https://github.com/anforaProject/anfora.git

Once you have cloned the repository we need to install the dependencies

    cd anfora
    pipenv install --three

This will create a virtual environement to work with python and automaticly it'll install all the packages needed.

### Creating a settings file 

Now we need to create the settings file (`An script to make easier this process will be included in future releases`). To do this we need to create a `.yaml` file with the necessary configuration:

Inside the folder `src/config` you have two examples of configurations. You can create a new copy and just override the values.

    cp src/config/dev.yaml src/config/my_config.yaml

Here you should pay attention to the http config and the storage config.
The media folder will contain all the media files and the `*_folder` are subfolders with different objetives.

Once you have your configuration file ready we need to crate the settings file. 

    pipenv run python src/commands.py -s --config src/config/my_config.yaml

This will change the configuration file at `src/settings.py`

### Installing the web client

Now we need to add the web client. For this we will clone as follow

    cd src
    git clone https://github.com/anforaProject/client.git

Now we need to compile the client files. To do this just type

    cd client
    yarn install
    yarn build

This will create a `dist` folder that is the one needed by the core to render the html.

## Configuring nginx
