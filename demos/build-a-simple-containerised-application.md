# Build a Simple Containerised Application

## Overview

This project showcases two containerised web applications built using Docker. The goal is to demonstrate hands-on proficiency in containerising both optimised and suboptimal web applications, highlighting the impact of Dockerfile best practices on image performance and efficiency.

### Key Concepts Demonstrated:
- Writing and optimising Dockerfiles  
- Building and managing Docker images  
- Running and exposing containers with appropriate port mapping  
- Comparing base image selection and layer efficiency  
- Cleaning up Docker resources to manage local development environments  

---

## Prerequisites

Ensure that you have cloned or downloaded the course GitHub repository to your local machine.

Navigate to the `build-a-simple-containerised-application` directory within the course repository.

---

## Application 1: 2048 Game

![2048 Diagram](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/2048_game.png)  
*Diagram: Illustrates the build process and deployment of the 2048 game using Docker.*

### Creating a Docker Image

1. Navigate to the `app1-2048` directory.  
2. Open the `Dockerfile` in your preferred text editor.

This Dockerfile consists of five key instructions: `FROM`, `LABEL`, `COPY`, `EXPOSE`, and `CMD`. Each contributes a layer during the Docker image build process.

Key points:

- **Base Image**: The build starts from the `nginx:latest` base image, which includes a preconfigured web server.  
- **Metadata**: A `LABEL` is used to define the image maintainer.  
- **Copying Files**: The `COPY` command transfers the contents of the local `2048/` directory into `/usr/share/nginx/html` inside the image.  
- **Port Exposure**: `EXPOSE 80` specifies that the application will be served on port 80.  
- **Container Startup**: The `CMD` sets the default command that runs when the container starts.

Build the Docker image by running:

```bash
docker build -t dockerized-2048 .
```

![Building Docker Image](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/building_image.png)

This process creates two layers:
- The base `nginx` image layer  
- A layer for the application files copied via the `COPY` instruction

### Running the Docker Container

1. List available images:

```bash
docker images
```

2. Identify the `dockerized-2048` image.

3. Run the container in detached mode with port mapping:

```bash
docker run -d -p 8081:80 dockerized-2048
```

- `-d`: Detached mode (runs in background)  
- `-p`: Maps port 8081 on the host to port 80 in the container

4. Verify the container is running:

```bash
docker ps
docker port <CONTAINER_ID>
```

![Running 2048 Container](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/ran_docker_image_2048.png)

5. Open a browser and visit [http://localhost:8081](http://localhost:8081) to access the game.

[Download .mov file](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/build-a-simple-containerised-application/2048_game_demo.mov)

### Cleanup

To stop and remove the container:

```bash
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

![Container Cleanup](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/container_cleanup.png)

---

## Application 2: Container of Cats

This example demonstrates a less optimised Dockerfile to highlight inefficiencies in image design.

### Creating the Docker Image

1. Navigate to the `app2-containerofcats` directory.  
2. Open the `Dockerfile`.

Differences from the previous example:

- **Base Image**: Uses `ubi8`, a general-purpose Red Hat UBI 8 image, as opposed to the more lightweight `nginx` image  
- **Installation Step**: Requires explicit installation of a web server (Apache) via a `RUN` command  
- **File Copying**: Two `COPY` instructions copy the `index.html` and JPEG assets into the image, creating additional layers

This approach results in a larger and slower-to-build image due to the less specialised base image and multiple layers.

Build the image with:

```bash
docker build -t containerofcats .
```

![Building Container of Cats](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/building_coc.png)

Despite a similar architecture (static files served via a web server), this image builds slower and consumes more resources.

### Running the Container

1. List available images:

```bash
docker images
```

2. Locate the `containerofcats` image.

3. Run the container with:

```bash
docker run -d -p 8081:80 containerofcats
```

4. Confirm the container is running:

```bash
docker ps
docker port <CONTAINER_ID>
```

![Running Container](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/ran_coc.png)

5. Visit [http://localhost:8081](http://localhost:8081) in your browser to view the application.

![Web Output – Cats](https://raw.githubusercontent.com/JThomas404/docker-course-adrian-cantrill/main/images/build-a-simple-containerised-application/coc_web_image.png)

### Cleanup

To stop and remove the container:

```bash
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
```

---