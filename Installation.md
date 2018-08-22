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
    pipenv install --python python3.6

This will create a virtual environement to work with python and automaticly it'll install all the packages needed.

### Creating a settings file 

Now we need to create the settings file (`An script to make easier this process will be included in future releases`). To do this we need to create a `.yaml` file with the necessary configuration:

Inside the folder `src/config` you have two examples of configurations. You can create a new copy and just override the values.

    cp src/config/dev.yaml src/config/my_config.yaml

Here you should pay attention to the http config and the storage config.
The media folder will contain all the media files and the `*_folder` are subfolders with different objetives.

Once you have your configuration file ready we need to crate the settings file. 
    
    cd src
    pipenv run python commands.py -s --config config/my_config.yaml

This will change the configuration file at `src/settings.py`

### Installing the web client

Now we need to add the web client. For this we will clone as follow

    git clone https://github.com/anforaProject/client.git

Now we need to compile the client files. To do this just type

    cd client
    yarn install
    yarn build

This will create a `dist` folder that is the one needed by the core to render the html.

### Creating the db

Anfora requires access to a PostgreSQL instance.

Create a user for a PostgreSQL instance:

#### Launch psql as the postgres user
    sudo -u postgres psql

#### In the following prompt
    CREATE USER anforaUser CREATEDB;
    \q

Note that we do not set up a password of any kind, this is because we will be using ident authentication. This allows local users to access the database without a password.

We'll need to sync the db. To do this just type

    cd ..
    pipenv run python commands.py -d
    
### Creating a new user

To create a user type

    pipenv run python create_user.py

And follow the instructions
 
## Configuring nginx

We need nginx to manage our conections with the world. 
Type `cd /etc/nginx/sites-available` and then `nano anfora.conf` (you can use your favorite editor).

Your configuration should be similar to:

    map $http_upgrade $connection_upgrade {
      default upgrade;
      ''      close;
    }

    # the upstream component nginx needs to connect to

    server {
        server_name www.anfora.test anfora.test;
        rewrite ^(.*) https://anfora.test$1 permanent;
    }
    # configuration of the server
    server {

        listen	         443;
        ssl                  on;

        server_name anfora.test;

        ssl_protocols TLSv1.2;
        ssl_ciphers HIGH:!MEDIUM:!LOW:!aNULL:!NULL:!SHA;
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:10m;

        ssl_certificate      /etc/ssl/certs/zinat.crt;
        ssl_certificate_key  /etc/ssl/private/zinat.key;



        keepalive_timeout    70;


        # the domain name it will serve for
        charset     utf-8;
        gzip on;
        gzip_disable "msie6";
        gzip_vary on;
        gzip_proxied any;
        gzip_comp_level 6;
        gzip_buffers 16 8k;
        gzip_http_version 1.1;
        gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;


        # max upload size
        client_max_body_size 75M;   # adjust to taste

        # Django media
        location /media/files  {
                 alias /home/anforaUser/uploads;  # Folder where you save the uploaded media
        }


        location /js {
                 alias /home/anforaUser/anfora/src/client/dist/js;
        }

        location /css {
                 alias /home/anforaUser/anfora/src/client/dist/css;
        }
        
        location /api/v1/streaming/ {
    	         proxy_pass http://127.0.0.1:4000;
                 proxy_buffering off;
                 proxy_redirect off;
                 proxy_http_version 1.1;
                 proxy_set_header Upgrade $http_upgrade;
                 proxy_set_header Connection $connection_upgrade;
                 tcp_nodelay on;


        }


        location / {
                 proxy_pass http://127.0.0.1:3000;
        }

    }

Here you should change:

* The `server_name` to match yours.
* Fix the paths in `alias`.
* Change the certs in `ssl_certificate`

We recomend [certbot](https://certbot.eff.org/) to manage your certs in production.

Save the changes and use

    ln -s ../sites-available/anfora.conf
    
to create a symbolic link to the config file so nginx can read it.
Finally restart nginx using `sudo nginx -s reload`.

## Starting services

The process that you need to start are:

* redis-server
* anfora server
* anfora node server
* huey consumer (tasks manager)

To continue I'll assume that you have an user called `anforaUser` and that the root of the project is at `home/anforaUser/anfora/`

WIP commands are available at [the project page](https://github.com/anforaProject/anfora#start-services)
