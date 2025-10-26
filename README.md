# 🐳 Docker Tutorial - From Beginner to Pro

Welcome to the **Complete Docker Learning Repository**!

This repo covers both **theory** and **hands-on examples** of Docker - designed for both beginners and experienced developers.

## What is Docker?

Docker is a containerization platform that allows us to package our applications and dependencies into containers - lightweight, portable, and isolated environments.

Think of it like this:
* Traditional apps run on OS directly -> prone to conflicts
* Docker apps run inside isolated "containers" -> consistent behavior everywhere

Example Analogy:
> If our app is a ship, Docker containers are like sealed cargo containers - they carry our goods (code + dependencies) safely across any ship (OS/environment).

## Why Use Docker?

* `Consistency:` Works the same on dev, staging, and production
* `Isolation:` No dependency conflicts
* `Portability:` Run anywhere (Windows, macOS, Linux, Cloud)
* `Scalability:` Ideal for microservices and cloud deployment
* `Efficiency:`Lightweight compared to virtual machines

## Key Docker Concepts

| Term           | Description                                |
| -------------- | ------------------------------------------ |
| **Image**      | Blueprint of our app (read-only template) |
| **Container**  | Running instance of an image               |
| **Dockerfile** | Script defining how an image is built      |
| **Volume**     | Persistent storage for containers          |
| **Network**    | Enables communication between containers   |
| **Registry**   | Repository for images (e.g., Docker Hub)   |

## Docker Architecture

```
+----------------------+
|     Docker CLI       |  <- We interact here
+----------+-----------+
           |
           v
+----------------------+
|   Docker Daemon      |  <- Core engine (runs containers, builds images)
+----------+-----------+
           |
           v
+----------------------+
|   Docker Objects     |
| (Images, Containers) |
+----------------------+

```

## Basic Docker Commands

| Command                   | Description                             |
| ------------------------- | --------------------------------------- |
| `docker --version`        | Check Docker version                    |
| `docker info`             | Show system-wide info                   |
| `docker images`           | List all images                         |
| `docker ps`               | List running containers                 |
| `docker ps -a`            | List all containers (stopped + running) |
| `docker pull <image>`     | Download an image                       |
| `docker run <image>`      | Run a container                         |
| `docker stop <container>` | Stop a running container                |
| `docker rm <container>`   | Remove a container                      |
| `docker rmi <image>`      | Remove an image                         |

## Working with Docker Images

### Pull an image from Docker Hub

```bash
docker pull nginx
```

### Run the image

```bash
docker run -d -p 8080:80 nginx
```

* `-d` -> detached mode (runs in background)
* `-p 8080:80` -> maps container port 80 to host port 8080

`Visit:` http://localhost:8080

### List images

```bash
docker images
```

### Remove image

```bash
docker rmi nginx
```

## Running & Managing Containers

### Run container with a name

```bash
docker run -d --name mynginx -p 8080:80 nginx
```

### Stop / Start

```bash
docker stop mynginx
docker start mynginx
```

### View logs

```bash
docker logs mynginx
```

### Execute command inside container

```bash
docker exec -it mynginx bash
```

`-it` -> interactive terminal

## Building our Own Image (Dockerfile)

### Example: Simple Python App

#### 1. app.py
```python
print("Hello from Docker!")
```

#### 2. Dockerfile
```Dockerfile
# Use base image
FROM python:3.9

# Set working directory
WORKDIR /app

# Copy source code
COPY app.py .

# Run app
CMD ["python", "app.py"]
```

#### Build & Run
```bash
docker build -t mypythonapp .
docker run mypythonapp
```

## Managing Containers
Learn how to control containers - start, stop, inspect, and remove them.

### List Containers
- Running containers:
```bash
docker ps
```

- All containers (including stopped):
```bash
docker ps -a
```

- Stop Container:
```bash
docker stop mynginx
```

- Start Container:
```bash
docker start mynginx
```

- Inspect Container:
```bash
docker inspect mynginx
```

- View Logs:
```bash
docker logs mynginx
```

- Access Container Shell:
```bash
docker exec -it mynginx bash
```

- Remove Container
```bash
docker rm mynginx
```

## Docker Volumes - Persistent Data
By default, Docker containers are **ephemeral** - their data disappears once the container is removed. **Volumes** solve this problem by storing data *outside* the container's writable layer.

### Common Commands

- Create a volume
```bash
docker volume create mydata
```

- List all volumes
```bash
docker volume ls
```

- Inspect a volume
```bash
docker volume inspect mydata
```

- Remove a volume
```bash
docker volume rm mydata
```

### Example: Persisting App Data

- data_writer.py

```python
import time

while True:
    with open("/data/output.txt", "a") as f:
        f.write("Log entry from Docker container\n")
    print("Wrote data to /data/output.txt")
    time.sleep(5)
```

- Dockerfile
```Dockerfile
FROM python:3.9
WORKDIR /app
COPY app/ .
CMD ["python", "data_writer.py"]
```

- Build and Run with a Volume
```bash
docker build -t data-writer .
docker run -d --name writer -v mydata:/data data-writer
```

- Check the Data
```bash
docker exec -it writer cat /data/output.txt
```

- Even if we remove the container:
```bash
docker rm -f writer
```

- The data still persists in the volume:
```bash
docker run -it --rm -v mydata:/data alpine cat /data/output.txt
```

Volume keeps our data safe!

## Docker Networking - Container Communication

Docker networking enables containers to communicate with each other securely and efficiently.

- List Networks
```bash
docker network ls
```

- Create Custom Network
```bash
docker network create mynet
```

- Inspect Network
```bash
docker network inspect mynet
```

### Network Types Overview

| Type        | Description                               |
| ----------- | ----------------------------------------- |
| **bridge**  | Default network for standalone containers |
| **host**    | Shares host's network stack               |
| **none**    | Isolated, no networking                   |
| **overlay** | Multi-host networking for Docker Swarm    |

> `Note:` When we use Docker Compose, it automatically creates a bridge network for all our services.