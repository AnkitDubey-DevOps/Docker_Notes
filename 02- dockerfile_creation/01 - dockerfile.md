# Docker & Dockerfile — Complete Guide

Let me walk you through everything you need to know, from the basics to pro tips.

---

# What is a Dockerfile?

A Dockerfile is a text file with instructions that tells Docker how to build an image. Think of it like a recipe — Docker follows it step by step to create a container image.

```text
Code → Dockerfile → Docker Image → Running Container
```

---

# Basic Structure

```Dockerfile
# Comment
INSTRUCTION argument
```

Every line is an instruction. Docker executes them top to bottom, and each instruction creates a new layer in the image.

---

# All Key Instructions Explained

## 1. FROM — The Base Image

```Dockerfile
FROM ubuntu:22.04
FROM node:20-alpine
FROM scratch          # Empty image (for Go binaries, etc.)
```

- Always the first instruction
- You're building ON TOP of an existing image
- `alpine` variants are tiny (5MB vs 200MB+) — prefer them for production

---

## 2. RUN — Execute Commands While Building

```Dockerfile
RUN apt-get update && apt-get install -y curl
RUN npm install
```

- Runs during image build time
- Each `RUN` creates a new layer — combine related commands with `&&`

### ✅ Good:

```Dockerfile
RUN apt-get update && apt-get install -y \
    curl \
    git \
    vim \
 && rm -rf /var/lib/apt/lists/*   # Clean up to reduce layer size
```

### ❌ Bad (creates 3 unnecessary layers):

```Dockerfile
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y git
```

## 3. COPY vs ADD — Bring Files In

```Dockerfile
COPY src/ /app/src/          # Copy local files into image
COPY package.json .          # Copy a specific file

ADD archive.tar.gz /app/     # ADD can auto-extract .tar.gz
ADD https://example.com/file /app/  # ADD can fetch URLs
```

Rule of thumb: Always use `COPY` unless you specifically need `ADD`'s extras. `COPY` is explicit and predictable.

---

## 4. WORKDIR — Set Working Directory

```Dockerfile
WORKDIR /app
COPY . .        # Now copies into /app
RUN npm install # Runs inside /app
```

- Like doing `mkdir + cd` in one step
- Always use this instead of `RUN cd /some/path`

---

## 5. ENV — Environment Variables

```Dockerfile
ENV NODE_ENV=production
ENV PORT=3000
ENV DB_HOST=localhost DB_PORT=5432   # Multiple on one line
```

- Available during build AND at runtime
- Override at runtime:

```bash
docker run -e NODE_ENV=staging myapp
```

---

## 6. ARG — Build-Time Variables

```Dockerfile
ARG VERSION=1.0
ARG APP_ENV=prod

RUN echo "Building version $VERSION"
```

- Only available during build, not at runtime
- Pass in:

```bash
docker build --build-arg VERSION=2.0 .
```

| Feature | ARG | ENV |
|---|---|---|
| Available at build time | ✅ | ✅ |
| Available at runtime | ❌ | ✅ |
| Override at build | `--build-arg` | `--build-arg` |
| Override at runtime | ❌ | `-e` flag |

---

## 7. EXPOSE — Document a Port

```Dockerfile
EXPOSE 3000
EXPOSE 8080/udp
```

- This is documentation only — it doesn't actually open ports
- Publish ports at runtime:

```bash
docker run -p 3000:3000 myapp
```

---

# Dockerfile: `COPY` vs `ADD`

| `COPY` | `ADD` |
|--------|--------|
| Copies files/directories from local machine to image | Copies files/directories **and** has extra features |
| Recommended for most use cases | Use only when needed |
| Does **not** extract `.tar` files | Automatically extracts local `.tar` files |
| Cannot download files from URLs | Can download files from URLs |

### Rule of Thumb
- ✅ Use **COPY** by default.
- ✅ Use **ADD** only for:
  - Extracting `.tar` files
  - Downloading files from a URL (rarely recommended)


# Dockerfile: `ARG` vs `ENV`

| `ARG` | `ENV` |
|-------|-------|
| Available **only during image build** | Available during **build and container runtime** |
| Can be changed using `--build-arg` | Stored in the image |
| Not available inside the running container | Available inside the running container |

### Rule of Thumb
- ✅ Use **ARG** for **build-time values** (e.g., version numbers).
- ✅ Use **ENV** for **runtime configuration** (e.g., PORT, DATABASE_URL).
