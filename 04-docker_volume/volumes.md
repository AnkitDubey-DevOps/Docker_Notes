## Docker Volume
it is a persistent data storage mechanism completely managed by docker allows containers to retain data independent of thier lifecycle.

### why use Docker Volume
1. Data Survives even if container is stop, delete or replace the container.
2. multiple running container can read/write to same volume simultaneously.


### Types of Docker Volume

#### Named Volume
This is a specific storage area that docker creates and manage for you. you give it a friendly, repeatable name so you can find it easily later.

We can create named volume like this

```
docker volume <volumename>
```

for using this volume while running container run this

```
docker run -v <volumename>:<dockerContainerPath> <imageName>
