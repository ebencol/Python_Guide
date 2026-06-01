# Docker

## What Is Docker?

Docker is a platform for packaging applications and their dependencies
into **containers**. Containers are lightweight, portable, and
consistent across environments.

### Benefits

-   Consistent development and production environments
-   Faster application deployment
-   Isolation between applications
-   Efficient resource usage

------------------------------------------------------------------------

## Core Concepts

### Image

A read-only template used to create containers.

### Container

A running instance of an image.

### Dockerfile

A text file containing instructions for building an image.

### Registry

A service that stores images, such as Docker Hub.

------------------------------------------------------------------------

## Installing Docker

Visit the official Docker website and install Docker Desktop for your
operating system:

-   Windows
-   macOS
-   Linux

After installation, verify:

``` bash
docker --version
```

------------------------------------------------------------------------

## Your First Container

Run a simple container:

``` bash
docker run hello-world
```

Docker will download the image if necessary and display a success
message.

------------------------------------------------------------------------

## Working with Images

Search for images:

``` bash
docker search nginx
```

Download an image:

``` bash
docker pull nginx
```

List local images:

``` bash
docker images
```

Remove an image:

``` bash
docker rmi nginx
```

------------------------------------------------------------------------

## Working with Containers

Start a container:

``` bash
docker run -d --name webserver -p 8080:80 nginx
```

Explanation:

-   `-d` = detached mode
-   `--name` = container name
-   `-p 8080:80` = map host port 8080 to container port 80

View running containers:

``` bash
docker ps
```

View all containers:

``` bash
docker ps -a
```

Stop a container:

``` bash
docker stop webserver
```

Start it again:

``` bash
docker start webserver
```

Remove a container:

``` bash
docker rm webserver
```

------------------------------------------------------------------------

## Viewing Logs

``` bash
docker logs webserver
```

Follow logs in real time:

``` bash
docker logs -f webserver
```

------------------------------------------------------------------------

## Executing Commands Inside a Container

Open a shell:

``` bash
docker exec -it webserver bash
```

Or:

``` bash
docker exec -it webserver sh
```

------------------------------------------------------------------------

## Building Your Own Image

Create a file named `Dockerfile`:

``` dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

Build the image:

``` bash
docker build -t my-python-app .
```

Run it:

``` bash
docker run -p 5000:5000 my-python-app
```

------------------------------------------------------------------------

## Volumes

Volumes persist data outside containers.

Create a volume:

``` bash
docker volume create mydata
```

Use it:

``` bash
docker run -v mydata:/data ubuntu
```

List volumes:

``` bash
docker volume ls
```

------------------------------------------------------------------------

## Networking

Create a network:

``` bash
docker network create mynetwork
```

Run containers on that network:

``` bash
docker run -d --network mynetwork --name app nginx
```

List networks:

``` bash
docker network ls
```

------------------------------------------------------------------------

## Docker Compose

Example `compose.yaml`:

``` yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: secret
```

Start services:

``` bash
docker compose up -d
```

Stop services:

``` bash
docker compose down
```

------------------------------------------------------------------------

## Useful Commands Cheat Sheet

``` bash
docker images
docker ps
docker ps -a
docker pull IMAGE
docker run IMAGE
docker stop CONTAINER
docker rm CONTAINER
docker logs CONTAINER
docker exec -it CONTAINER bash
docker build -t NAME .
docker compose up -d
docker compose down
```

------------------------------------------------------------------------

## Best Practices

1.  Use small base images.
2.  Keep images immutable.
3.  Avoid running as root.
4.  Use `.dockerignore`.
5.  Pin dependency versions.
6.  Scan images for vulnerabilities.
7.  Use multi-stage builds.
