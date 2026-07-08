## Docker Volume
it is a persistent data storage mechanism completely managed by docker allows containers to retain data independent of thier lifecycle.

### why use Docker Volume
1. Data Survives even if container is stop, delete or replace the container.
2. multiple running container can read/write to same volume simultaneously.


### Types of Docker Volume

#### Named Volume
This is a specific storage area that docker creates and manage for you. you give it a friendly, repeatable name so you can find it easily later.

it is best for databases, persistent application data or things you want to share between multiple container.

We can create named volume like this

```
docker volume <volumename>
```

for using this volume while running container run this

```
docker run -v <volumename>:<dockerContainerPath> <imageName>
```

if the <volumename> you mentioned exists, docker will use that volume, if it doesn't exist then docker will create a volume based on the name and will run container with it.

### Anonymous Volume
it is exactly like named volume but you do not give it a name. instead docker will automatically generates a long, confusing string of numbers and letter as its ID.

it is best for temporary storage or logs that you dont care to track closely.

