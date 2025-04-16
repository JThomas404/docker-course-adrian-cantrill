# 🐳 Docker Fundamentals — Learning by Doing

Welcome to the **Docker Fundamentals** repo — a hands-on companion to learning Docker based on [Adrian Cantrill's Docker course](https://learn.cantrill.io/p/docker-fundamentals).

This repo is designed for **learning by doing** — each section contains interactive, practical demonstrations of key Docker concepts, helping you go from beginner to confident user.

---

## 🧪 Demos

A growing collection of Docker demonstrations with real commands, screenshots, and results:

| Demo | Description |
|------|-------------|
| [Working with Existing Docker Images](./demos/working-with-existing-docker-images.md) | Pull, inspect, and run containers using public images from Docker Hub |
| [Docker Container Volumes](./demos/docker-container-volumes.md) | Use bind mounts and volumes to persist container data |
| [Docker Container Environment Variables](./demos/docker-container-environment-variables.md) | Pass environment variables to containers at runtime |
| [Build a Simple Containerised Application](./demos/build-a-simple-containerised-application.md) | Build a Docker image for a basic app using a custom Dockerfile |
| [Docker Compose](./demos/docker-compose.md) | Define and manage multi-container applications using `docker-compose.yml` |

*More demos coming soon...*

---

## 🧠 Topics Covered

- Installing Docker
- Pulling & inspecting images
- Running containers (attached & detached)
- Port mapping
- Volumes and data persistence
- Environment variables
- Dockerfiles and image building
- Docker Compose
- Container lifecycle commands
- Interacting with containers (`exec`, logs, restart)
- Clean-up strategies
- ...and more to come

---

## 🚀 Getting Started

Clone the repo:

```bash
git clone https://github.com/JThomas404/docker-course-adrian-cantrill.git
cd docker-course-adrian-cantrill
```

Make sure Docker is installed and running on your machine. Then dive into the `demos/` folder and start experimenting!

---

## 🙏 Credits

This repo was created while following along with [Adrian Cantrill's Docker Fundamentals course](https://learn.cantrill.io/p/docker-fundamentals).

**PS**: Much of the structure and examples are adapted from Adrian's course repo, with additions and images for enhanced hands-on learning.

---