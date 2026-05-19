# Docker Installation and important commands
Create a machine on AWS with Docker installed AMI, and install Docker if
not installed. yum install docker

## To see all images present in your local

```
docker images

```

## To find out images in the docker hub.

```

docker search <image_name>

```

## To download an image from docker-hub to a local machine

```

docker pull <image_name>

```

## To check, whether the service is starting or not.

```

service docker status or service docker info

```

## To start the service

```

service docker start

```

## To stop the service

```

service docker stop

```

## To go inside the container

```

docker attach <conatiner_name>

```

## To see all the conatiner

```

docker ps -a

```

## To see only running container

```

docker ps

```

## To delete the container and images

```

docker rm <container_name>
docker image rm <image_name>

```
