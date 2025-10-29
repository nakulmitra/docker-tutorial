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

## What is Docker Compose?

Docker Compose is a **tool for defining and running multi-container Docker applications**. If Docker helps you run one container easily, **Docker Compose** helps you manage **entire stacks** - like a web server, database, and cache **with one command**.

You describe your app's **services, networks, and volumes** in a file called:
```bash
docker-compose.yml
```

Then, with one command:
```bash
docker-compose up
```

Docker Compose will automatically:

* Build images (if needed)
* Create containers
* Connect them with networks
* Manage dependencies

## Why Use Docker Compose?

| Problem                              | Solution with Docker Compose |
| ------------------------------------ | ---------------------------- |
| Running multiple containers manually | Define all in one file       |
| Managing environment variables       | Centralized config           |
| Linking containers                   | Automatic networking         |
| Restarting stacks                    | `docker-compose restart`     |
| Rebuilding images                    | `docker-compose build`       |

## Docker Compose File Structure

A basic `docker-compose.yml` looks like this:

```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
```

### Breakdown:

* **version:** Compose file format version (usually "3")
* **services:** List of containers to run
* **image:** Docker image name
* **ports:** Host:Container port mapping
* **environment:** Environment variables passed into container

## Example Project - Node.js + MongoDB

Let's create a real-world setup with two services:

* **node-app** - Backend API
* **mongo-db** - Database

### Step 1 - server/index.js

```javascript
const express = require("express");
const mongoose = require("mongoose");
const app = express();

mongoose.connect("mongodb://mongo-db:27017/docker_demo")
  .then(() => console.log("Connected to MongoDB"))
  .catch(err => console.error(err));

app.get("/", (req, res) => {
  res.send("Hello from Node.js and MongoDB with Docker Compose!");
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

### Step 2 - server/package.json

```json
{
  "name": "docker-compose-node-mongo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^8.0.0"
  }
}
```

### Step 3 - server/Dockerfile

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### Step 4 - docker-compose.yml

```yaml
version: "3.8"

services:
  node-app:
    build: ./server
    ports:
      - "3000:3000"
    depends_on:
      - mongo-db
    environment:
      - MONGO_URL=mongodb://mongo-db:27017/docker_demo

  mongo-db:
    image: mongo:6
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

## Step 5 - Run the App

Start everything:

```bash
docker-compose up -d
```

Check running containers:

```bash
docker ps
```

View logs:

```bash
docker-compose logs -f
```

Visit your app at: [http://localhost:3000](http://localhost:3000)

You should see:

```
Hello from Node.js and MongoDB with Docker Compose!
```

## Stopping and Cleaning Up

Stop containers but keep data:

```bash
docker-compose down
```

Remove everything including volumes:

```bash
docker-compose down -v
```

## Useful Docker Compose Commands

| Command                               | Description                     |
| ------------------------------------- | ------------------------------- |
| `docker-compose up`                   | Create and start containers     |
| `docker-compose up -d`                | Run containers in detached mode |
| `docker-compose down`                 | Stop and remove containers      |
| `docker-compose build`                | Build or rebuild images         |
| `docker-compose logs`                 | View output from services       |
| `docker-compose ps`                   | List running services           |
| `docker-compose exec <service> <cmd>` | Run command inside container    |

## Networking in Compose

* Docker Compose automatically creates a **network** for our app.
* All services can communicate using **service names** as hostnames.

Example:

```javascript
mongoose.connect("mongodb://mongo-db:27017/docker_demo")
```

Here, `mongo-db` is the service name, not an IP address.

## Persistent Data with Volumes

Volumes in `docker-compose.yml` persist your data even when containers are removed.

Example:

```yaml
volumes:
  mongo-data:
```

This ensures MongoDB data stays safe between container restarts.