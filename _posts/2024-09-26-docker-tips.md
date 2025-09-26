---
layout: post
title: "Essential Docker Commands Every Developer Should Know"
date: 2024-09-26
---

Docker has revolutionized how we develop and deploy applications. Here are some essential commands that will make your Docker workflow more efficient:

## Container Management

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Start a container
docker start container_name

# Stop a container gracefully
docker stop container_name

# Remove a container
docker rm container_name

# Run a container interactively
docker run -it ubuntu bash
```

## Image Management

```bash
# List local images
docker images

# Pull an image from Docker Hub
docker pull nginx:latest

# Build an image from Dockerfile
docker build -t my-app:latest .

# Remove an image
docker rmi image_name

# Remove unused images
docker image prune
```

## Useful One-Liners

```bash
# Stop all running containers
docker stop $(docker ps -q)

# Remove all stopped containers
docker rm $(docker ps -a -q)

# Remove all unused images, containers, and networks
docker system prune -a
```

## Docker Compose Quick Reference

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    volumes:
      - .:/app
      - /app/node_modules
```

Run with:
```bash
docker-compose up -d
```

These commands will cover 90% of your daily Docker needs. Master these, and you'll be containerizing like a pro!

## Video Tutorial

Check out this comprehensive Docker tutorial:

<iframe width="560" height="315" src="https://www.youtube.com/embed/fqMOX6JJhGo" title="Docker Tutorial for Beginners" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Happy containerizing! 🐳
