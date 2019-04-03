# Docker

## Understand and be able to explain the following concepts

### What is Docker

Docker is a technology or software that allows us to manipulate and build containers easily.

### How are containers different from virtual machines?

Containers run on the host's kernel whereas virtual machines run on emulated hardware.

### What is the difference between the Docker CLI and the Docker engine?

Actually, the Docker engine is composed a these three components: the Docker daemon (`dockerd`), the Docker CLI (`docker`) and a REST API with which programs (including the CLI) can communicate with `dockerd`.

### Why can we say that Docker is based on a client-server architecture?

We can say it's a client-server architecture because the client (`docker`) sends commands to the server (`dockerd`) through the REST API.

### What is the difference between a Docker image and a Docker container?

A Docker image is a read-only template with instructions for creating and running a Docker container.

### How does one create a Docker image?

A Docker image is created using the `docker build` command which uses a file named Dockerfile. This file contains all necessary instructions to build the image.

### How does one create a Docker container?

A Docker container is created using the `docker run` command which takes the name of an image as argument.

### What is Dockerhub?

Dockerhub is a repository of Docker images.

### Explain what happens when you type `docker run -it --rm alpine /bin/sh`.

With this command we run the `/bin/sh` command inside a new container created from the `alpine` image. `-i` and `-t` means we want an interactive pseudo-TTY and `--rm` means we want the container to be automatically removed when it exits.

### Explain how port mapping works in Docker.

By default, a container connects to the bridge network and exposes all of its ports to other containers, but none to the outside world. This is done using the `--publish`or `-p` option.

### Explain how to use the `-p xx:yy` option when using `docker run`.

`xx` represents the localhost port we want to use to connect to the `yy` port of the container. Whilst we can chose the value for `xx` we cannot chose the value for `yy`, it is defined by the Dockerfile (using `EXPOSE`) and the software running inside the container.

## Be able to perform the following operations

### Write a Dockerfile to define an image that contains a TCP server written in Java

```Dockerfile
FROM java:8
COPY my-server.jar /somewhere/my-server.jar
```

### Run multiple containers from the same image

```sh
docker run [options] <image-name> [command]
```

### Obtain the IP address assigned to the each of these containers

```sh
docker inspect <container-name> | grep IPAddress
```

### Send requests to the TCP server running in the containers, with nc or telnet

Use `-p` option when running the container and then do `nc localhost <port-no>`.

### Log into a running container and explore the file system

```sh
docker exec -it <container-name> /bin/sh
```