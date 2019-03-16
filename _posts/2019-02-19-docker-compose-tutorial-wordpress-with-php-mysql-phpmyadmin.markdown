---
layout: post
title:  "Docker compose tutorial: WordPress with PHP, MySQL, phpMyAdmin"
date:   2019-02-20 00:00:00 +0700
language: en
categories: devops
tags:
    - devops
    - docker
    - docker compose
    - wordpress
    - php
    - mysql
---


In this tutorial, I will instroduce you about docker compose in practice.

Firstly, I will show you a full configration for this tutorial. It is quite simple. And after that, I will explain each section.

```yaml
version: "3"

networks:
  wordpress-network:

services:
  mysql:
    image: mysql:5.6
    networks:
      - wordpress-network
    hostname: mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=123123

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    networks:
      - wordpress-network
    hostname: phpmyadmin
    restart: always
    ports:
      - 8080:80
    environment:
      - PMA_HOST=mysql

  php-apache:
    build:
      context: .
      dockerfile: Dockerfile-php
    networks:
      - wordpress-network
    hostname: wordpress
    volumes:
      - ./wordpress:/var/www/html/
    restart: always
    ports:
      - 80:80
```

There are three parts in this file. The first section is the version of docker file, you can see [here](https://docs.docker.com/compose/compose-file/) to have more information about it.

The second is the networks. In this file, I had defined one network with name is **wordpress-network**.

The last section is services. In each service, you should notice:

- **image** attribute is the name of docker image in docker hub or other registries; in networks attribute, this contains the network which container uses.
- **hostname** is used for other service call a request without ip, it like a domain, for more information you should read [here](https://docs.docker.com/compose/compose-file/#extra_hosts).
- In **volumes** attribute, I have defined a mapping folder from host to container.
- In **ports** attribute, I have defined a mapping port from host to container.
- **restart** with always mean this container likes a service, for more information, you should see [here](https://docs.docker.com/compose/compose-file/#restart).
- Sometime, you must modify some image or create a new image. In this case, **build** attribute will help you. You should provide a docker for new image and a context which build command need.

For completed code, you can visite [here](https://github.com/HanhThong/demo-blog/tree/docker-compose-for-php-wordpress-phpmyadmin-mysql).

For video demo, you can visite [here](https://www.youtube.com/watch?v=HMFfqTPn1V8)