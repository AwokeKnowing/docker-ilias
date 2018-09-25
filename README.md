# About this Repo

This is a fork of the repository of the studer + raimann ag Docker image for
[ILIAS](https://www.ilias.de). See [the Docker Hub
page](https://hub.docker.com/r/sturai/ilias/) for information on how to use
this Docker image.

to build (tested on ubuntu 16.04):

`docker build --rm --no-cache -t awokeknowing/ilias:5.3 -t awokeknowing/ilias:5.3-apache-php7 -t awokeknowing/ilias:latest 5.3/apache-php7`


### run 
to run, first run a mysql container, and link it

```
docker run --name iliasdb -d \
  --restart unless-stopped \
  -e MYSQL_ROOT_PASSWORD=ilias123 \
  -e MYSQL_DATABASE=ilias \
  mysql:5.7  --character-set-server=utf8 --collation-server=utf8_general_ci

```

wait 30 seconds then run:

#### without ssl

```
docker run --name ilias -d \
  --link iliasdb:mysql \
  -e ILIAS_DB_USER=root \
  -e ILIAS_DB_PASSWORD=ilias123 \
  -p 80:80 \
  awokeknowing/ilias:5.3-apache-php7
```

#### with ssl

setup your default-ssl.conf then mount the certificate 

```
docker run --name ilias -d \
  --link iliasdb:mysql \
  -e ILIAS_DB_USER=root \
  -e ILIAS_DB_PASSWORD=ilias123 \
  -p 80:80 \
  -p 443:443 \
  -v /etc/ssl/certs/__mydomain_com.crt:/etc/ssl/certs/__mydomain_com.crt:ro  \
  -v /etc/ssl/certs/__mydomain_com.ca-bundle:/etc/ssl/certs/__mydomain_com.ca-bundle:ro  \
  -v /etc/ssl/certs/private.key:/etc/ssl/certs/private.key:ro  \
  -v default-ssl.conf:/etc/apache2/sites-enabled/default-ssl.conf \
  awokeknowing/ilias:5.3-apache-php7
```
