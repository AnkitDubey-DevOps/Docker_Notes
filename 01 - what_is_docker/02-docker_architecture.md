# Docker Concepts

<h2>Docker Architecture</h2>

<img 
  src="https://docs.docker.com/get-started/images/docker-architecture.webp" 
  alt="Docker Architecture" 
  width="100%"
/>

## 1. Docker Daemon

The Docker Daemon is the main background service of Docker.  
It manages Docker containers, images, networks, and volumes.  
It listens to Docker commands and performs the required actions.

### Simple Meaning:
Docker Daemon is the engine that runs Docker.

---

## 2. Docker Client

The Docker Client is the command-line tool used by users to interact with Docker.  
It sends commands to the Docker Daemon.

### Examples:
```bash
docker run
docker build
docker pull
```

### Simple Meaning:
Docker Client is the interface used to communicate with Docker.

---

## 3. Docker Desktop

Docker Desktop is an application used to run Docker on Windows and macOS.  
It includes Docker Daemon, Docker Client, Docker Compose, Kubernetes, and GUI tools.

### Features:
- Easy installation
- GUI support
- Container management
- Development tools

### Simple Meaning:
Docker Desktop is the complete Docker application for PCs.

---

## 4. Docker Registries

Docker Registry is a storage location where Docker images are stored and shared.

### Popular Registry:
- Docker Hub

### Example:
```bash
docker pull nginx
```

### Simple Meaning:
Docker Registry is like an online store for Docker images.

---

## 5. Docker Objects

Docker Objects are the components used in Docker to run applications.

### Main Docker Objects:
- Images
- Containers
- Networks
- Volumes

### Simple Meaning:
Docker Objects are the building blocks of Docker.

---

## 6. Docker Images

A Docker Image is a read-only template used to create containers.

### It contains:
- Application code
- Libraries
- Dependencies
- Environment settings

### Example:
```bash
docker pull ubuntu
```

### Simple Meaning:
Docker Image is the blueprint of a container.

---

## 7. Docker Container

A Docker Container is a running instance of a Docker image.

Containers are lightweight, portable, and isolated environments used to run applications.

### Features:
- Fast execution
- Lightweight
- Portable
- Secure

### Simple Meaning:
Container is the actual running application created from an image.

---

# Difference Between Image and Container

## Image:
- Blueprint or template
- Static
- Read-only

## Container:
- Running instance of image
- Dynamic
- Executable

### Example:
```text
Image = Recipe
Container = Prepared Food
```

# Example docker run command

The following command runs an ubuntu container, attaches interactively to your local command-line session, and runs `/bin/bash`.

```bash
docker run -i -t ubuntu /bin/bash
```

When you run this command, the following happens (assuming you are using the default registry configuration):

1. If you don't have the ubuntu image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.

2. Docker creates a new container, as though you had run a `docker container create` command manually.

3. Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem.

4. Docker creates a network interface to connect the container to the default network, since you didn't specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine's network connection.

5. Docker starts the container and executes `/bin/bash`. Because the container is running interactively and attached to your terminal (due to the `-i` and `-t` flags), you can provide input using your keyboard while Docker logs the output to your terminal.

6. When you run `exit` to terminate the `/bin/bash` command, the container stops but isn't removed. You can start it again or remove it.
