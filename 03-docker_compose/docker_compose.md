# Docker Compose (Expert Notes)

## What is Docker Compose?

**Docker Compose** is a tool used to define and manage **multi-container Docker applications** using a single configuration file (`compose.yaml` or `docker-compose.yml`).

Instead of running multiple `docker run` commands manually, Docker Compose lets you define all containers, networks, volumes, environment variables, and dependencies in one YAML file and manage them with a single command.

---

# Why Do We Need Docker Compose?

Imagine a web application that consists of:

- Frontend (React)
- Backend (Node.js)
- Database (MySQL)
- Redis Cache
- Nginx Reverse Proxy

Without Compose, you would have to:

- Create a network
- Start MySQL
- Start Redis
- Start Backend
- Start Frontend
- Start Nginx
- Connect all containers
- Mount volumes
- Configure environment variables

This requires multiple `docker run` commands.

With Docker Compose:

```bash
docker compose up
```

Everything starts automatically.

---

# Docker Compose Architecture

```
compose.yaml
        │
        ▼
 Docker Compose CLI
        │
        ▼
 Docker Engine
        │
        ▼
Creates:
- Containers
- Networks
- Volumes
- Environment Variables
```

---

# Main Components of Docker Compose

## 1. Services

A **service** represents one container (or multiple identical containers).

Example:

```yaml
services:
  web:
    image: nginx

  db:
    image: mysql
```

Here:

- `web` → Nginx container
- `db` → MySQL container

---

## 2. Networks

Compose automatically creates a private network.

Example:

```yaml
services:
  backend:
    image: node

  database:
    image: mysql
```

Backend can connect using:

```
database:3306
```

No IP address is needed.

Docker provides automatic DNS.

---

## 3. Volumes

Volumes store persistent data.

Without volumes:

```
Delete container
↓

Database is deleted
```

With volumes:

```
Delete container
↓

Data remains
```

Example:

```yaml
volumes:
  mysql-data:
```

Attach volume:

```yaml
services:
  db:
    volumes:
      - mysql-data:/var/lib/mysql
```

---

## 4. Environment Variables

Used for configuration.

Example:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: root
  MYSQL_DATABASE: appdb
```

---

## 5. Build

Instead of pulling an image, Compose can build it.

```yaml
services:
  backend:
    build: .
```

or

```yaml
build:
  context: .
  dockerfile: Dockerfile
```

---

# Basic Structure of compose.yaml

```yaml
services:
  web:
    image: nginx

  app:
    build: .

volumes:
  data:

networks:
  backend:
```

---

# Example Project

```
project/

│
├── Dockerfile
├── compose.yaml
├── app.py
└── requirements.txt
```

compose.yaml

```yaml
services:

  app:
    build: .
    ports:
      - "5000:5000"

  mysql:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
```

Start everything:

```bash
docker compose up
```

---

# Common Docker Compose Commands

## Start Containers

```bash
docker compose up
```

---

## Start in Background

```bash
docker compose up -d
```

---

## Stop Containers

```bash
docker compose stop
```

---

## Stop and Remove Containers

```bash
docker compose down
```

---

## Rebuild Images

```bash
docker compose up --build
```

---

## View Running Containers

```bash
docker compose ps
```

---

## View Logs

```bash
docker compose logs
```

Specific service:

```bash
docker compose logs web
```

---

## Follow Logs

```bash
docker compose logs -f
```

---

## Restart Services

```bash
docker compose restart
```

---

## Execute Command Inside Container

```bash
docker compose exec web bash
```

---

## Scale Containers

```bash
docker compose up --scale web=3
```

Creates:

```
web_1
web_2
web_3
```

---

# Important Compose Keywords

## image

Uses an existing image.

```yaml
image: nginx:latest
```

---

## build

Builds image from Dockerfile.

```yaml
build: .
```

---

## ports

Maps host port to container port.

```yaml
ports:
  - "8080:80"
```

Meaning:

```
Host:8080
↓

Container:80
```

---

## volumes

Mount storage.

```yaml
volumes:
  - ./src:/app
```

or

```yaml
volumes:
  - mysql-data:/var/lib/mysql
```

---

## environment

Sets environment variables.

```yaml
environment:
  NODE_ENV: production
```

---

## depends_on

Starts containers in dependency order.

```yaml
services:

  backend:
    depends_on:
      - database

  database:
    image: mysql
```

**Note:** `depends_on` only controls startup order. It does **not** wait until the database is ready to accept connections.

---

## restart

Restart policy.

```yaml
restart: always
```

Options:

```
no
always
on-failure
unless-stopped
```

---

## container_name

Assigns a custom container name.

```yaml
container_name: my-nginx
```

---

## command

Overrides the default command.

```yaml
command: python app.py
```

---

## working_dir

Sets the working directory.

```yaml
working_dir: /app
```

---

## env_file

Loads environment variables from a file.

```yaml
env_file:
  - .env
```

---

## networks

Connects services to a network.

```yaml
networks:
  backend:
```

---

# Docker Compose Networking

Compose automatically creates a network.

Example:

```
Frontend
      │
      ▼
Backend
      │
      ▼
MySQL
```

Compose creates:

```
project_default
```

All services join this network automatically.

Containers communicate using **service names**, for example:

```text
mysql
redis
backend
```

instead of IP addresses.

---

# Named Volumes vs Bind Mounts

## Named Volume

```yaml
volumes:
  - mysql-data:/var/lib/mysql
```

- Managed by Docker
- Best for databases
- Persists even if the container is removed

## Bind Mount

```yaml
volumes:
  - ./src:/app
```

- Maps a host directory into the container
- Ideal for development
- Code changes on the host are reflected immediately

---

# Environment Variables

Using `.env`

```
PORT=5000
DB_PASSWORD=root
```

compose.yaml

```yaml
services:
  app:
    env_file:
      - .env
```

or

```yaml
environment:
  PORT: ${PORT}
```

---

# Docker Compose Lifecycle

```
docker compose up
      │
      ▼
Read compose.yaml
      │
      ▼
Build images (if required)
      │
      ▼
Create network
      │
      ▼
Create volumes
      │
      ▼
Create containers
      │
      ▼
Start containers
```

---

# Advantages of Docker Compose

- Define an entire application in a single YAML file
- Start multiple containers with one command
- Automatic network creation
- Automatic DNS (service name resolution)
- Easy volume management
- Easy environment variable management
- Supports image building
- Simplifies local development and testing
- Easy to share application setup with a team
- Supports scaling services

---

# Limitations

- Primarily designed for a **single Docker host**
- Not intended for large-scale production orchestration
- Limited auto-scaling and self-healing capabilities
- For distributed production environments, **Kubernetes** or **Docker Swarm** are more suitable

---

# Interview Questions

### Q1. What is Docker Compose?

A tool to define and manage multi-container Docker applications using a single YAML configuration file.

---

### Q2. Difference between Docker and Docker Compose?

| Docker | Docker Compose |
|---------|----------------|
| Manages individual containers | Manages multiple containers together |
| Uses `docker run` | Uses `docker compose up` |
| Manual networking | Automatic networking |
| Manual volume creation | Defined in YAML |
| Best for single containers | Best for multi-container applications |

---

### Q3. What file does Docker Compose use?

- `compose.yaml` (recommended)
- `docker-compose.yml` (legacy but still supported)

---

### Q4. What is `depends_on`?

Starts dependent services before the current service. It **does not** guarantee that the dependent service is fully ready.

---

### Q5. How do containers communicate in Docker Compose?

Through the default Compose network using **service names** as hostnames (e.g., `mysql`, `redis`, `backend`).

---

### Q6. Difference between `docker compose stop` and `docker compose down`?

| `stop` | `down` |
|----------|---------|
| Stops containers only | Stops and removes containers |
| Network remains | Network is removed |
| Volumes remain (unless specified otherwise) | Anonymous volumes are removed if requested with `-v` |
