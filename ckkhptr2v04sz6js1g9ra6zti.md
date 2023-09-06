---
title: "Tech bytes# Running Jenkins in a Docker Container"
datePublished: Fri Jan 29 2021 03:17:12 GMT+0000 (Coordinated Universal Time)
cuid: ckkhptr2v04sz6js1g9ra6zti
slug: tech-bytes-running-jenkins-in-a-docker-container
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1611562182114/pQSLmoaEf.jpeg
tags: docker, continuous-integration, containers, jenkins

---

Would you like to use Jenkins in your machine and don't want to follow the traditional installation process ? If your say YES - then follow along this brief article !

### Prerequisites:

This article assumes you have some idea of what *docker* is and how to use it. The pre-requisites to get started are:

- `Docker` running in your machine

> If you are completely new to docker, I would highly recommend starting from [here](https://www.docker.com/get-started).

### Installation Steps:

- Create a folder called **build** and add a `Dockerfile` in it with content as below.

```
# build/Dockerfile
FROM jenkins/jenkins:lts-alpine

USER root 
RUN apk add docker
``` 

- Create a `docker-compose.yml` as below:

```
#docker-compose.yml

version: "3.8"

services:
  jenkins:
    build: build/
    ports: 
      - 8090:8080
    volumes:
      - "jenkins.data:/var/jenkins_home"
      - "/var/run/docker.sock:/var/run/docker.sock"

volumes:
  jenkins.data:
```

 - Run the below command to start the service:

```
docker-compose up -d
```
- Open the browser at:

```
localhost:8090
```

- We get a jenkins screens as below for the initial administrator password 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611555641082/nJXPFP65x.png)

- Get the password from:

```
docker-compose exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

- After entering the password from above step, Jenkins shows the plugin selection screen.


![Jenkins Plugin Screen](https://cdn.hashnode.com/res/hashnode/image/upload/v1611555746384/_YNUE0SJN.png)

After the successful installation of the required plugins you will be redirected to Jenkins Dashboard page. You may create an admin user if required.


![Jenkins Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1611561981662/16REaAS6p.png)

That's it. You can use Jenkins now to create and run jobs as long as the container is running :) 

