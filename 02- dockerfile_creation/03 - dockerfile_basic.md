# Dockerfile

- A Dockerfile is indeed a text file containing a set of instructions. These instructions define how to build a Docker image step by step.
- Dockerfiles automate the process of creating Docker images, ensuring consistency and reproducibility in the deployment of applications.

---

# Steps for Dockerfile Usage

1. Create a File Named `Dockerfile`
2. Add Instructions in the Dockerfile
3. Build a Dockerfile to Create an Image
4. Run the Image to Create a Container

---

# The Dockerfile supports the following instructions:

| Instruction | Description |
|---|---|
| FROM | Create a new build stage from a base image. |
| RUN | Execute build commands. |
| MAINTAINER | Specify the author of an image. |
| COPY | Copy files and directories. |
| ADD | Add local or remote files and directories. |
| EXPOSE | Describe which ports your application is listening on. |
| WORKDIR | Change working directory. |
| CMD | Specify default commands. |
| ENTRYPOINTS | Specify default executable. |
| ENV | Set environment variables. |
| VOLUME | Create volume mounts. |
| USER | Set user and group ID. |
| LABEL | Add metadata to an image. |
| ONBUILD | Specify instructions for when the image is used in a build. |
| SHELL | Set the default shell of an image. |
| ARG | Use build-time variables. |
| HEALTHCHECK | Check a container's health on startup. |
| STOPSIGNAL | Specify the system call signal for exiting a container. |

---

# Our First Dockerfile

```bash
$ vi Dockerfile
```

```Dockerfile
FROM Ubuntu
RUN echo "Learning Docker" > /tmp/testfile
```

```bash
:wq
```

```text
(echo "Learning Docker") > /tmp/testfile
it means write "Learning Docker" in file name testfile in tmp folder
```

---

# To create image out of Dockerfile

```bash
$ docker build -t <image_name> .
```

Example:

```bash
docker build -t learningDocker .
```

```text
-t = use for tag
. (dot) Use the current directory to build a Docker image using the Dockerfile located in the current directory
```

Now you can see image by using:

```bash
$ docker images
```

Also you can try to create a container from this image 😊

Now you can play with different Dockerfile and try to do experiment by yourself 😊

---

# Dockerfile Build Flow

```text
Dockerfile → Build → Docker Image → Run → Docker Container
```
