# Build a Simple Containerised Application

## Overview

This project demonstrates the creation and optimisation of two containerised web applications using Docker. The objective is to show practical proficiency in:

- Writing efficient Dockerfiles
- Managing Docker images and containers
- Applying Dockerfile best practices to reduce image size and build time
- Understanding the effects of different base image selections and layer strategies
- Managing Docker resources to maintain clean development environments

---

## Prerequisites

Before proceeding, clone or download the course GitHub repository.

Navigate to the `build-a-simple-containerised-application` directory.

---

## Application 1: 2048 Game

![2048 Diagram](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/2048_game.png)  
*Diagram: Illustrates the build process and deployment of the 2048 game using Docker.*

### Creating the Docker Image

1. Navigate to `app1-2048`.
2. Open the `Dockerfile`.

The Dockerfile contains the following key instructions: `FROM`, `LABEL`, `COPY`, `EXPOSE`, and `CMD`.

**Highlights:**

- **FROM**: Uses `nginx:latest` as a pre-configured web server.
- **LABEL**: Adds metadata.
- **COPY**: Moves local files to `/usr/share/nginx/html` in the container.
- **EXPOSE**: Declares port 80 for external access.
- **CMD**: Specifies default command for the container.

Build the Docker image:

```bash
docker build -t dockerized-2048 .
```

![Building Image](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/building_image.png)

This creates:

- A base image layer (`nginx`)
- An application layer (`COPY`)

### Running the Docker Container

1. List images:

```bash
docker images
```

2. Identify the `dockerized-2048` image.
3. Start the container:

```bash
docker run -d -p 8081:80 dockerized-2048
```

4. Confirm it’s running:

```bash
docker ps
docker port <CONTAINER_ID>
```

![Running Container](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/ran_docker_image_2048.png)

5. Open [http://localhost:8081](http://localhost:8081) in your browser.

📽️ [Download .mov file](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/2048_game_demo.mov)

### Cleanup

```bash
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

![Cleanup](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/container_cleanup.png)

---

## Application 2: Container of Cats

This second application intentionally uses a less optimised Dockerfile to demonstrate inefficiencies.

### Creating the Docker Image

1. Navigate to `app2-containerofcats`.
2. Review the `Dockerfile`.

**Differences:**

- **Base Image**: `ubi8`, a larger Red Hat image
- **RUN**: Manually installs Apache
- **COPY**: Two separate instructions for `index.html` and images

Build the image:

```bash
docker build -t containerofcats .
```

![Building Image](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/building_coc.png)

This version is slower to build and larger due to a less efficient base image and multiple layers.

### Running the Container

```bash
docker run -d -p 8081:80 containerofcats
```

Verify it’s active:

```bash
docker ps
docker port <CONTAINER_ID>
```

![Running Container](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/ran_coc.png)

Access it at [http://localhost:8081](http://localhost:8081)

![Rendered Site](https://raw.githubusercontent.com/dennisbyrne/containerisation-course-docs/main/docs/build-a-simple-containerised-application/images/coc_web_image.png)

### Cleanup

```bash
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

---