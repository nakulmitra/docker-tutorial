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

```
docker pull nginx
```

### Run the image

```
docker run -d -p 8080:80 nginx
```

* `-d` -> detached mode (runs in background)
* `-p 8080:80` -> maps container port 80 to host port 8080

`Visit:` http://localhost:8080

### List images

```
docker images
```

### Remove image

```
docker rmi nginx
```

## Running & Managing Containers

### Run container with a name

```
docker run -d --name mynginx -p 8080:80 nginx
```

### Stop / Start

```
docker stop mynginx
docker start mynginx
```

### View logs

```
docker logs mynginx
```

### Execute command inside container

```
docker exec -it mynginx bash
```

`-it` -> interactive terminal

## Building Your Own Image (Dockerfile)

### Example: Simple Python App

#### 1. app.py
```
print("Hello from Docker!")
```

#### 2. Dockerfile
```
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
```
docker build -t mypythonapp .
docker run mypythonapp
```