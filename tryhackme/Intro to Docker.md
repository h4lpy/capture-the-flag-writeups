# Intro to Docker

**images**: instructions for what a container *should* execute


```Dockerfile
# THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```

Container is built and run with:

```
$ docker build -t webserver
$ docker run -d --name webserver -p 80:80 webserver
```

## Docker Compose

Allows multiple containers (or apps) to interact with each other while running in isolation from each other.

![](Pasted%20image%2020250614135359.png)

Apps require different services to run which cannot be accomplished in a single container. Docker Compose allows us to create multiple microservices/applications as one singular service/application.

```yml
version: '3.3'
services:
  web:
    build: ./web
    networks:
      - ecommerce
    ports:
      - '80:80'


  database:
    image: mysql:latest
    networks:
      - ecommerce
    environment:
      - MYSQL_DATABASE=ecommerce
      - MYSQL_USERNAME=root
      - MYSQL_ROOT_PASSWORD=helloword
    
networks:
  ecommerce:
```

Docker is client/server. Docker interacts 

![](Pasted%20image%2020250615081232.png)