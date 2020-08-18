# About this repository
 This repository is contained Dockerfile and material for building images of Nginx on official Ubuntu docker images.

## How to use this image

There are some methods to use this image.


### Launch a nginx container with a simple command.

1. Launch a container with default setting.

  ```
  $ docker run --name [my-nginx] -d nmoeini/nginx:1.16-xenial
  ```

2. Launch a container with static content.

  ```
  $ docker run --name [my-nginx] -v /my/static/content:/data/www -d nmoeini/nginx:1.16-xenial
  ```

3. Launch a container and exposing the port.

  ```
  $ docker run --name [my-nginx] -p 8080:80 -d nmoeini/nginx:1.16-xenial
  ```
  Then you can access `http://localhost:8080` or `http://host-ip:8080` in your browser.

4. Launch a container with config file.

  ```
  $ docker run --name [my-nginx] -v /my/nginx.conf:/etc/nginx/nginx.conf:ro -d nmoeini/nginx:1.16-xenial
  ```

  `:ro` It's define this directory is `read-only` in container.


### Launch a nginx container with Dockerfile.

* Step one: Create a simple Dockerfile like the following:

  ```
  FROM nmoeini/nginx:1.16-xenial

  COPY /my/static/content /data/www
  COPY /my/nginx.conf /etc/nginx/nginx.conf
  COPY /my/conf.d/default.conf /etc/nginx/conf.d/default.conf
  ```
* Step two: Build the docker image with this Dockerfile.

  ```
  $ docker build -t [my-ubuntu-nginx] .
  ```

* Run the docker image with the following command:

  ```
  $ docker run --name [my-nginx] -d [my-ubuntu-nginx]
  ```

### Launch a nginx container with Docker-Compose

* Step one: Create a simple docker-compose.yml file:

  ```
  my-nginx:
    image: nmoeini/nginx:1.16-xenial
    volumes:
      - /my/static/content:/data/www
      - /my/nginx.conf:/etc/nginx/nginx.conf
      - /my/conf.d/default.conf:/etc/nginx/conf.d/default.conf
    ports:
      - "8080:80"
    environment:
      - NGINX_HOST=example.com
      - NGINX_PORT=80
    command: /bin/bash -c "nginx -g 'daemon off;'"
  ```

* Step two: Build and run this docker-compose.yml with Docker-Compose command.

  ```
  $ docker-compose run my-nginx
  ```


## Reference

* [Nginx Official site](http://nginx.org/)
* [Nginx Config](http://nginx.org/en/docs/beginners_guide.html)
