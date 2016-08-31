# Docker images for Guacamole 0.99 on tomcat 9.0.0-jre8 mysql-connector-java-6.0.2

Run [Guacamole](http://guac-dev.org/), the clientless remote desktop gateway inside Docker containers.

- Daemon: [danielguerra/guacamole-guacd](https://registry.hub.docker.com/u/danielguerra/guacamole-guacd/)
- Database backend: [danielguerra/guacamole-db](https://registry.hub.docker.com/u/danielguerra/guacamole-db/)
- Web application: [danielguerra/guacamole-webserver](https://registry.hub.docker.com/u/danielguerra/guacamole-webserver/)

Updated clone from mattgruter https://github.com/mattgruter/dockerfile-guacamole

## Getting started
To run the Guacamole daemon, web application and a database backend for authentication do:

    docker run --name guacd danielguerra/guacamole-guacd
    docker run --name db danielguerra/guacamole-db
    docker run --link guacd:guacd --link db:db -p 8080:8080 danielguerra/guacamole-webserver

Now point your browser at [http://localhost:8080](http://localhost:8080).

The default user is `guacadmin` with password `guacadmin`.


## Docker-compose
If you use [docker-compose](https://docs.docker.com/compose/) you can build and start all containers with:

    docker-compose up

And point your browser at [http://localhost:8080](http://localhost:8080).

The default user is `guacadmin` with password `guacadmin`.

## Fig
If you use [fig](http://www.fig.sh/) you can bulid and start all containers with:

    fig up

Or if you don't want to build the images yourself and use the prebuild images from the Docker Hub:

    fig -f fig.prod.yml up

And point your browser at [http://localhost:8080](http://localhost:8080).

The default user is `guacadmin` with password `guacadmin`.


## Daemon
To only run the Guacamole daemon:

    docker run danielguerra/guacamole-guacd

The guacd default port `4822` is exposed by the image.


## Database backend
To only run a Guacamole-ready MariaDB server:

    docker run danielguerra/guacamole-db

The MariaDB server exposes it's default port `3306`.


## Web application
To only run the Guacamole web application:

    docker run -p 8080:8080 guacd danielguerra/guacamole-webserver

The web application expects a running [guacd](https://github.com/danielguerra/dockerfile-guacamole/tree/master/guacd) Guacamole daemon at the address `guacd:4822` and a [Guacamole-ready MySQL database server](https://github.com/danielguerra/dockerfile-guacamole/tree/master/db) at `db:3306`.
You'll probably want to start a guacd and database container first and then link to them as described above.
