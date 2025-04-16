Certainly! Below is your Markdown formatted with the image links as per your request:

```markdown
# Project: Building Simple Containerised Applications with Docker

## Overview

This project involved building and running two simple containerised web applications using Docker. The goal was to gain practical experience with containerisation concepts, Dockerfiles, image layering, and running lightweight versus heavier containers. The applications included:

1. **2048 Game** – a static web game deployed via an NGINX container.
2. **Container of Cats** – a static HTML page served via an Apache server on a UBI8 base image.

---

## Application 1: 2048 Game

### Objective

Package a static HTML + JavaScript game (2048) using a lightweight web server (`nginx`) in a Docker container and expose it locally via port 8081.

---

### Dockerfile Breakdown

Location: `app1-2048/Dockerfile`

```Dockerfile
FROM nginx:latest
LABEL maintainer="your.email@example.com"
COPY 2048/ /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Key points:**

- `nginx:latest` is used as the base image for serving static content.
- Application files are copied into the default NGINX HTML directory.
- Port 80 is exposed for web traffic.
- The container runs NGINX in the foreground using the standard CMD.

---

### Build and Run Instructions

```bash
cd app1-2048
docker build -t dockerized-2048 .
```

**Build output:**  
![2048 Build Output](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/2048-build.png)

```bash
docker run -d -p 8081:80 dockerized-2048
```

**Running container:**  
![2048 Container Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/2048-running.png)

Open the application in a browser at `http://localhost:8081`.

**Accessed in browser:**  
![2048 in Browser](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/2048-browser.png)

---

### Outcome

- The 2048 game loaded instantly and worked without issues.
- The final image size was minimal (~20–30 MB) thanks to the efficiency of the `nginx` base image.
- This setup demonstrated best practices for serving static files with Docker.

---

## Application 2: Container of Cats

### Objective

Package a static HTML site with image assets using a general-purpose Red Hat UBI 8 image and serve it via Apache (`httpd`). This was intentionally less efficient to learn how Docker image size and layering work.

---

### Dockerfile Breakdown

Location: `app2-containerofcats/Dockerfile`

```Dockerfile
FROM registry.access.redhat.com/ubi8/ubi
RUN yum -y install httpd && yum clean all
COPY index.html /var/www/html/index.html
COPY images/ /var/www/html/images/
EXPOSE 80
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

**Key Points:**

- Uses `ubi8` (Universal Base Image 8) as a more general-purpose container base.
- Apache is installed manually via `yum`, creating a heavier image.
- The HTML and image files are manually copied into the correct Apache directory.
- Port 80 is exposed, and Apache is run in the foreground.

---

### Build and Run Instructions

```bash
cd app2-containerofcats
docker build -t containerofcats .
```

**Build output:**  
![Container of Cats Build Output](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/containerofcats-build.png)

```bash
docker run -d -p 8081:80 containerofcats
```

**Running container:**  
![Container of Cats Container Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/containerofcats-running.png)

Open the application in a browser at `http://localhost:8081`.

**Accessed in browser:**  
![Container of Cats in Browser](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/containerofcats-browser.png)

---

### Outcome

- Successfully deployed a container serving an HTML site with images.
- Compared with the 2048 project, this container had a longer build time and significantly larger size due to:
  - The use of `ubi8` instead of `nginx`.
  - Additional layer from installing Apache.
  - Multiple `COPY` instructions.

This illustrated why leaner base images and minimal layers are ideal in production environments.

---

## Key Learnings

- **Image Size Matters**: Lightweight images (like `nginx`) are significantly more efficient than general-purpose ones.
- **Layering**: Every instruction in the Dockerfile creates a new image layer. Consolidating `RUN` and `COPY` commands can reduce image size.
- **Static Web Servers**: For purely static content, using a web server like NGINX is more efficient than installing a full HTTP stack.
- **Container Port Mapping**: Mapping host-to-container ports (`-p 8081:80`) is essential for exposing local web services.

---

## Commands Reference

```bash
# Build
docker build -t <tag> .

# Run and expose
docker run -d -p <host_port>:<container_port> <tag>

# List running containers
docker ps

# Stop and remove container
docker stop <container_id>
docker rm <container_id>
```

---