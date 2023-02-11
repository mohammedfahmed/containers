## docker commands

```shell
docker run [-d -it -p h:c -v h:c -e]
docker ps [-a]
docker inspect NAME/ID
docker stop NAME/ID
docker logs
docker rm NAME/ID
docker images
docker rmi IMAGENAME
docker pull IMAGENAME
docker exec NAME COMMAND
docker exec -it NAME /bin/bash
docker attach NAME/ID
docker detach
docker container ls
docker network ls
docker system prune
docker image prune
docker volume prune
docker network prune
docker build > push
```

## docker-compose commands

```shell
docker-compose start
docker-compose stop

docker-compose pause
docker-compose unpause

docker-compose ps
docker-compose --compatibility -f FILENAME up -d
docker-compose down
```

## Dockerfile commands 

```shell
FROM creates a layer from the ubuntu:18.04 Docker image.
MAINTAINER
RUN executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.
CMD sets default command and/or parameters, which can be overwritten from command line when docker container runs, there must be only one CMD
LABEL
EXPOSE only indicates what port the container will be listening on, it doesn't automatically open the port on the container
ENV
ADD
COPY adds files from your Docker client’s current directory.
ENTRYPOINT allows you to configure a container that will run as an executable. It looks similar to CMD, because it also allows you to specify a command with parameters. The difference is ENTRYPOINT command and parameters are not ignored when Docker container runs with command line parameters.
VOLUME
USER
WORKDIR
ARG
ONBUILD
STOPSIGNAL
HEALTHCHECK
SHELL
EXPOSE 
WORKDIR
USER
ENV
```


## Clean up

```shell
# List all containers (only IDs)

docker ps -aq

# Stop all running containers

docker stop $(docker ps -aq)

# Remove all containers

docker rm $(docker ps -aq)

# Remove all images

docker rmi $(docker images -q)
```
## Network drivers

- bridge 
- host
- overlay
- macvlan
- none

##  Sample file
```yaml
# docker-compose.yml
version: '2'

services:
  web:
    build: .
    # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```


## Building

```yaml
web:
  # build from Dockerfile
  build: .
  args:     # Add build arguments
    APP_HOME: app

  # build from custom Dockerfile
  build:
    context: ./dir
    dockerfile: Dockerfile.dev

  # build from image
  image: ubuntu
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry:4000/postgresql
  image: a4bc65fd
```


## Other options

```yaml
  # make this service extend another
  extends:
    file: common.yml  # optional
    service: webapp

  volumes:
    - /var/lib/mysql
    - ./_data:/var/lib/mysql
```


## Ports

```yaml
  ports:
    - "3000"
    - "8000:80"  # host:container

  # expose ports to linked services (not to host)
  expose: ["3000"]
```

## Commands

```yaml
  # command to execute
  command: bundle exec thin -p 3000
  command: [bundle, exec, thin, -p, 3000]

  # override the entrypoint
  entrypoint: /app/start.sh
  entrypoint: [php, -d, vendor/bin/phpunit]
```

## Environment variables

```yaml
  # environment vars
  environment:
    RACK_ENV: development
  environment:
    - RACK_ENV=development

  # environment vars from file
  env_file: .env
  env_file: [.env, .development.env]
```

## Dependencies

```yaml
  # makes the `db` service available as the hostname `database`
  # (implies depends_on)
  links:
    - db:database
    - redis

  # make sure `db` is alive before starting
  depends_on:
    - db
```


## Labels

```yaml
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

## DNS servers

```yaml
services:
  web:
    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 8.8.4.4
```

## Devices

```yaml
services:
  web:
    devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
```

## External links

```yaml
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

## Hosts

```yaml
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```

## Network

```yaml
# creates a custom network called `frontend`
networks:
  frontend:
```

## External network

```yaml
# join a pre-existing network
networks:
  default:
    external:
      name: frontend
```

## Volume

```yaml
# Mount host paths or named volumes, specified as sub-options to a service
  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  dbdata:

```



## exec vs. run

docker exec: executes a command on a running container. 

docker run: creates a temporary container, executes the command in it and stops the container when it is done.
