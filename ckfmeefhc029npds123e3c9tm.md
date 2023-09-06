---
title: "Getting started with deploying PostgreSQL on Docker container"
datePublished: Mon Sep 28 2020 07:48:24 GMT+0000 (Coordinated Universal Time)
cuid: ckfmeefhc029npds123e3c9tm
slug: getting-started-with-deploying-postgresql-on-docker-container
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1601286598782/--1D0TxYO.jpeg
tags: postgresql, docker

---

Have you ever felt that installing software is hard? It might become complex because of missing dependencies of the complex applications and many configurations around it. It requires lot of effort from developer side, takes time to check the errors, resolve the conflicts and to have the software up & running. [Docker](https://www.docker.com/) provides a simple way of installing and running the applications with an easy to follow API. In this article we are going to understand the process of installing and running the `postgreSQL` using docker. You can follow along these steps for having `postgres` installed for your local development as well as remote server setup.

## Contents

[Introduction](#introduction)

[Pre-requisites](#pre-requisites)

[Option - 1: Running Postgres with direct docker commands](#option-1-running-postgres-with-direct-docker-commands)

[Option - 2: Running Postgres with docker compose](#option-2-running-postgres-with-docker-compose)

[Running multiple instances of postgres in the same host](#running-multiple-instances-of-postgres-in-the-same-host)

[Conclusion](#conclusion)

## Introduction

PostgreSQL is an open-source, object-relational database management system (ORDBMS). It is also commonly referred as *Postgres*. As a database server, its primary function is to store data, supporting best practices and retrieve it later. Developers opt for this relational database as it is free, stable and flexible.

Deploying Postgres in a container is cost-efficient in terms of infrastructure, it also supports CI/CD development, and streamlines deployment and application management.

## Pre-requisites
This article assumes you have some idea of what `docker` is and how to use it. The pre-requisites to get started are:

- Docker daemon running in your machine
- Account has been created in [docker hub](https://hub.docker.com/)

> If you are completely new to `docker`, I would highly recommend starting from [here](https://www.docker.com/get-started).


We could either choose to run `postgres` with direct docker commands or use a declarative `docker-compose` file

___

## Option - 1: Running Postgres with direct docker commands

### Pull Postgres image from Docker Hub
To download a particular image, or set of images (i.e., a repository), use `docker pull`. If no tag is provided, Docker Engine uses the :latest tag as a default. The `docker pull` command syntax is

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```
Run the below command on **terminal** to pull the `postgres` image from docker hub

```
$ docker pull postgres
```
You would see the below outcome if the action is successful

```
$ docker pull postgres
Using default tag: latest
latest: Pulling from library/postgres
d121f8d1c412: Already exists
9f045f1653de: Pull complete
fa0c0f0a5534: Pull complete
54e26c2eb3f1: Pull complete
cede939c738e: Pull complete
69f99b2ba105: Pull complete
218ae2bec541: Pull complete
70a48a74e7cf: Pull complete
c0159b3d9418: Pull complete
353f31fdef75: Pull complete
03d73272c393: Pull complete
8f89a54571bf: Pull complete
4885714928b5: Pull complete
3060b8f258ec: Pull complete
Digest: sha256:0171a93d62342d2ab2461069609175674d2a1809a1ad7ce9ba141e2c09db1156
Status: Downloaded newer image for postgres:latest
docker.io/library/postgres:latest
```

You could verify the local cache of the images available on the system using the below command.
```
$ docker images
REPOSITORY                       TAG                 IMAGE ID            CREATED             SIZE
postgres                         latest              817f2d3d51ec        2 days ago          314MB
```

### Start the Postgres container

Now that the `postgres` image is on our system, we need to use the `docker run` command to start the container using the image that we just downloaded. The syntax for `docker run` is as follows

```
docker run --name [container_name] -e POSTGRES_PASSWORD=[your_password] -d postgres
```

Create a docker volume as below
```
$ docker volume create --name=pgdata
```
Use the below command to start the `postgres` container using the `postgres:latest` image we just downloaded. We run it with couple of parameters like port, volumes etc.
```
$ docker run --rm --name pg-docker -e POSTGRES_PASSWORD=password -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/pgdata  postgres
```

After running the above command we should be getting a hash value of the container just started. As you observe, we have provided various options to the `docker run` command:

- *--rm: This informs the docker to automatically remove the container and it’s associated file system upon exit. If we are running so many short term containers, it is a good practice to pass `rm` flag to the docker run command for automatic cleanup and avoid disk space issues(this will be a life saver!). We can anyway use the `-v` option to persist data beyond the lifecycle of a container*
- *--name: An identifier for the container. We can choose any name we want. Note that two existing (even if they are stopped) containers cannot have the same name. In order to re-use a name, you would either need to pass the `rm` flag to the docker run command or explicitly remove the container by using the command `docker rm [container name]`*
- *-e: Exposes environment variable of name `POSTGRES_PASSWORD` with value `password` to the container. This environment variable sets the superuser password for PostgreSQL. We can set `POSTGRES_PASSWORD` to anything we like. There are additional environment variables you can set. These include `POSTGRES_USER` and `POSTGRES_DB`. `POSTGRES_USER` sets the superuser name. If not provided, the superuser name defaults to `postgres`. `POSTGRES_DB` sets the name of the default database to setup. If not provided, it defaults to the value of `POSTGRES_USER`*
- *-d: Launches the container in a detached mode or in other words, in the background - so that we could use the terminal for other operations*
- *-p: Binds port `5432` on localhost to port `5432` within the container. This option enables applications running out side of the container to be able to connect to the Postgres server running inside the container*
- *-v: Mounts `$HOME/docker/volumes/postgres` on the host machine to the container side volume path `/var/lib/postgresql/pgdata` created inside the container - here `pgdata` is the docker volume that we created earlier. This ensures that `postgres` data persists even after the container is removed*

> There are other options also available for `docker run`. For a complete list of options please check [here](https://docs.docker.com/engine/reference/run/)

Verify the active instance of the `postgres` server using:
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
62f6afbe7802        postgres            "docker-entrypoint.s…"   3 seconds ago       Up 2 seconds        0.0.0.0:5432->5432/tcp   pg-docker
```

### Connect to the postgres DB

The command syntax to connect to any container is:

```
docker exec -it [container_name] psql -U [postgres_user]
```

- ### If you are using these steps in a windows machine

First we need to connect to the `postgres` container in-order to do some work

```
$ docker exec -it pg-docker bash
root@62f6afbe7802:/#
```

As you have observed, now we have root access to the container. You may also notice the container ID that we have been using since starting.

Now we change the user to `postgres` which is the default super user created by default for us.
```
$ root@62f6afbe7802:/# su postgres
postgres@62f6afbe7802:/$
```

Once logged in with the `postgres` user, run the `psql` command as below

```
$ postgres@62f6afbe7802:/$ psql
psql (13.0 (Debian 13.0-1.pgdg100+1))
Type "help" for help.
```

Now we are in the `psql` interactive session, so that we could use all the `postgres` related activities.

For example to check the connection information issue the below command
```
$ postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
```
To create a new database with name `testdb` run this

```
$ postgres=# create database testdb;
CREATE DATABASE
```
In-order to list all the available databases use this
```
$ postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 testdb    | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
(4 rows)

postgres=# 
```

As you observe, the newly create database `testdb` is present in the list. Similarly you can run other `psql` commands and use the DB for development and management tasks.

Use exit multiple times to come out of different shells

```
$ postgres=# exit
postgres@62f6afbe7802:/$ exit
exit
root@62f6afbe7802:/# exit
exit
$
```

> Please refer [this](https://www.postgresqltutorial.com/psql-commands/) for some of the commonly used actions within `psql`

You can also connect with a GUI client like Dbweaver, PgAdmin etc.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601282861907/SQCpdeAWe.png)


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601282942329/w_4_kdZd9.png)

> Please note that you may observe issues while connecting to `postgres` server running in docker container in windows. As per the [issue](https://github.com/sameersbn/docker-postgresql/issues/112) we need to first stop the existing `postgres` window service if any, to proceed with login.
 
- ### If you are using these steps in a linux machine

Follow these steps for Linux machine - most of them are similar with minor modifications (*tested in Ubuntu 16.04 LTS*)

```
$ sudo docker exec -it pg-docker psql -U postgres
psql (13.0 (Debian 13.0-1.pgdg100+1))
Type "help" for help.

$ postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
postgres=# create database testdb;
CREATE DATABASE
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 testdb    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
(4 rows)

postgres=# 
```

> We could also just run the `docker run` command as above, without having to pull the image separately. Docker will pull the required image if its not present in the local cache

___

## Option - 2: Running Postgres with docker compose

Instead of running all the docker commands individually, we can leverage `docker-compose` file to start the `postgres` service. It provides us a way to specify all the required configuration parameters declaratively in a file.

> Ensure you have the docker-compose installed and available. It not please follow the steps [here](https://docs.docker.com/compose/install/) to get it

### Create a docker-compose file
- To ensure an easy and clean installation, we first create a new folder named `postgres` and move into that folder
```
$ mkdir postgres
$ cd postgres
```

- Next we use the docker-compose file to download the postgres image and get the service up and running. Using your favorite utility to create a YAML file as below(using nano/powershell)
```
$ nano docker-compose.yml
```
OR 
```
$ ni docker-compose.yml
```
- Add the below content to the `docker-compose` file and save

```
version: "3.8"

services:
  db:
    image: postgres
    restart: always
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - "5432:5432"
volumes:
  pgdata:
    external: true
```

The YAML configuration outlines the below:

- *version:  file format version for `docker-compose`*

- *services: defines a service in compose file with name `db`*

- *image: uses the `postgres` image that uses the tag `latest` by default*

- *restart: specifies a restart policy for how a container should or should not be restarted on exit.*

- *environment: used to specify the environment variables used for starting the services*

- *ports: 5432 is the default port number for PostgreSQL. We map the port 5432 from container to the same port on host machine*

- *volumes: directive that mounts the source directories or volumes from host machine at target paths inside the container.*

> Please ensure to use the correct YAML file syntax. Would suggest to use the Visual Studio code to validate the file syntax before starting the service.

### Start the postgres container with docker-compose

- Run the below command to start the container using the `-d` flag

```
$ docker-compose up -d
```

- You can check the logs with the command

```
$ docker-compose logs -f
```

That's it! Now you are ready to use the `postgres` DB in your development activities. Use the same steps as mentioned above to connect to the DB.

Finally, use the below to stop the `postgres` service

```
$ docker-compose stop
```


> In-order to find the required image in the repository use the below steps

### Search for a specific image in docker hub

Once you sign-up for the docker hub account, you can start using those credentials in terminal. Issue the below command to login to docker hub with your user details.
```
$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: rakeshvardan
Password:
Login Succeeded
```

If you are not familiar with the image name, you can even search the entire docker public repository as below and choose the appropriate images(always suggested to use official images from respective product teams)
```
$ docker search postgres
NAME                                    DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
postgres                                The PostgreSQL object-relational database sy…   8383                [OK]
```

___

## Running multiple instances of postgres in the same host

We can start multiple postgres containers in the same host and use different versions of the servers for local development.

```
$ docker run --rm  --name pg-docker-dev -e POSTGRES_PASSWORD=devpassword -d -p 5432:5432 -v $HOME/docker/volumes/postgresdev:/var/lib/postgresql/pgdata  postgres
```

```
$ docker run --rm  --name pg-docker-qa -e POSTGRES_PASSWORD=qapassword -d -p 5433:5432 -v $HOME/docker/volumes/postgresqa:/var/lib/postgresql/pgdata  postgres
```

As you can see, we started 2 different `postgres` databases for 2 environments - one for *development* and another for *testing*. Once started you can use these DB's independently of each other. We have also mounted two different folders(`postgresdev` and `postgresqa`) for each of the database to persist the data in the host machine. A different port `5433` on the host is mapped for the qa DB. You can also use two different versions of `postgres` on the same machine if you have a requirement to do so.

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
48b82d7dc83f        postgres            "docker-entrypoint.s…"   14 seconds ago      Up 12 seconds       0.0.0.0:5433->5432/tcp   pg-docker-qa
e9814264368a        postgres            "docker-entrypoint.s…"   42 seconds ago      Up 40 seconds       0.0.0.0:5432->5432/tcp   pg-docker-dev
```

## Conclusion

I hope that now you got a basic idea on how to get started using `postgres` with `docker`. So next time whenever you are trying to install any software try checking for respective docker images and start using them. You will have lot of fun.

Thank you !

### References
- [Docker hub repo for postgreSQL](https://hub.docker.com/_/postgres)
- [How To Deploy PostgreSQL On Docker Container](https://phoenixnap.com/kb/deploy-postgresql-on-docker#:~:text=Running%20PostgreSQL%20on%20Docker%20Containers,-Deploying%20a%20Postgres&text=The%20first%20option%20uses%20Docker,file%20with%20all%20the%20specifications.)
- [Don’t install Postgres. Docker pull Postgres](https://hackernoon.com/dont-install-postgres-docker-pull-postgres-bee20e200198)

