# ns8-checkmk

[CheckMK](https://checkmk.com) is an IT monitoring platform.

## Install

Install via Software Center by adding my repo as explained [here](https://repo.mrmarkuz.com)

## Configure

Configure the FQDN in the app settings.

Point your browser to the FQDN and login with username cmkadmin and the password cmkadmin.

## Documentation

- [CheckMK Documentation](https://docs.checkmk.com)

## Uninstall

To uninstall the instance:

    remove-module --no-preserve checkmk1

## Debug

some CLI are needed to debug

- The module runs under an agent that initiate a lot of environment variables (in /home/checkmk1/.config/state), it could be nice to verify them
on the root terminal

    `runagent -m checkmk1 env`

- you can become runagent for testing scripts and initiate all environment variables
  
    `runagent -m checkmk1`

 the path become : 
```
    echo $PATH
    /home/checkmk1/.config/bin:/usr/local/agent/pyenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/
```

- if you want to debug a container or see environment inside
 `runagent -m checkmk1`
 ```
podman ps
CONTAINER ID  IMAGE                                      COMMAND               CREATED        STATUS        PORTS                    NAMES
d292c6ff28e9  localhost/podman-pause:4.6.1-1702418000                          9 minutes ago  Up 9 minutes  127.0.0.1:20015->80/tcp  80b8de25945f-infra
d8df02bf6f4a  docker.io/library/mariadb:10.11.5          --character-set-s...  9 minutes ago  Up 9 minutes  127.0.0.1:20015->80/tcp  mariadb-app
9e58e5bd676f  docker.io/library/nginx:stable-alpine3.17  nginx -g daemon o...  9 minutes ago  Up 9 minutes  127.0.0.1:20015->80/tcp  checkmk-app
```

you can see what environment variable is inside the container
```
podman exec  checkmk-app env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM=xterm
PKG_RELEASE=1
MARIADB_DB_HOST=127.0.0.1
MARIADB_DB_NAME=checkmk
MARIADB_IMAGE=docker.io/mariadb:10.11.5
MARIADB_DB_TYPE=mysql
container=podman
NGINX_VERSION=1.24.0
NJS_VERSION=0.7.12
MARIADB_DB_USER=checkmk
MARIADB_DB_PASSWORD=checkmk
MARIADB_DB_PORT=3306
HOME=/root
```

you can run a shell inside the container

```
podman exec -ti   checkmk-app sh
/ # 
```
