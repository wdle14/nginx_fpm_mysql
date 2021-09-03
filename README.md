# docker-php-nginx-redis-mysql

PHP 7.4
Mysql Latest

Clone This Project 

```bash
git clone https://github.com/wdle14/nginx_fpm_mysql.git
```

This is a backend stack of decoupled technologies in docker containers.
If you have installed docker-compose only use

```bash
sudo docker-compose up --build
```
it uses a HA proxy, which is for ease of use of multiple domains and scaling of containers based on domains.

first you need to define the domain name on local computer

For Linux

```bash
vi /etc/hosts
```

For Windows
```bash
C:\Windows\System32\drivers\etc\hosts
```

add to the line of the file with sampel local domain name `local.test`

```bash
127.0.0.1 local.test
```

create a folder in the main folder to place the laravel project

```bash
sudo mkdir webapp
cd webapp
git clone https://github.com/{Your Laravel Project}
```

this docker file supports laravel environment

for some reason you have a problem with limited web server resources in handling many requests, then you just need to do scaling on the web server

```bash
docker-compose scale php-web={number scaling}
```

remove everything (volumes, images, etc.)

```bash
sudo docker-compose down --rmi all -v --remove-orphans
```

HTTP/HTTPS proxy

The Docker daemon uses the HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environmental variables in its start-up environment to configure HTTP or HTTPS proxy behavior. You cannot configure these environment variables using the daemon.json file. read more [docker systemd](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)


Otherwise please follow the steps in the [official docker documentation](https://docs.docker.com/install/linux/docker-ce/debian/) to install docker compose

## Thanks :)
