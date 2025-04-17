# **Working with Existing Docker Images**

This guide walks you through how to pull and manage Docker images, run containers, and interact with them using best practices. Whether you're a developer or a hiring manager evaluating technical skills, this document provides a straightforward, hands-on introduction to Docker.

---

## **1. Checking Your Docker Environment**

Before launching or managing containers, it’s important to understand your current Docker environment.

### View Running Containers

```bash
docker ps
```

Displays all containers currently running on your host. Useful for identifying active applications and services.

### List Available Docker Images

```bash
docker images
```

Shows all locally stored images, regardless of whether they’re currently in use. These images can be used to create new containers.

---

## **2. Pulling an Image from Docker Hub**

Docker Hub is the default registry for publicly available Docker images.

### Step 1: Find Your Image

- Navigate to [hub.docker.com](https://hub.docker.com/)
- Search for the required image (e.g., `containerofcats`)

### Step 2: Pull the Image

```bash
docker pull acantril/containerofcats
```

This command downloads the `acantril/containerofcats` image to your local system.

![Image Pull](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/pulled_docker_image.png)

---

## **3. Running a Container from an Image**

With the image downloaded, you can now start a container.

### Start a Container with Port Mapping

```bash
docker run -p 8081:80 acantril/containerofcats
```

This maps port `80` inside the container to port `8081` on your host machine. Open your browser and go to:

```
http://localhost:8081
```

![Container Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/container_of_cats.png)

---

## **4. Running Containers in Detached Mode**

Detached mode allows containers to run in the background, keeping your terminal free.

### Run in Background (Detached)

```bash
docker run -p 8081:80 -d acantril/containerofcats
```

The `-d` flag ensures the container runs in the background.

### Confirm It’s Running

```bash
docker ps
```

Use this to verify that your container is running in detached mode.

---

## **5. Interacting with Containers**

Once your container is running, you can execute commands or troubleshoot issues.

### Run a Command Inside the Container

```bash
docker exec -it <CONTAINER_ID> ps aux
```

Replace `<CONTAINER_ID>` with the actual container ID. This example shows running processes.

### Open an Interactive Shell

```bash
docker exec -it <CONTAINER_ID> sh
```

Opens a shell inside the container so you can navigate the file system or run interactive commands.

---

## **6. Viewing Container Logs**

Logs provide insight into your container’s runtime behaviour.

### View Logs

```bash
docker logs <CONTAINER_ID>
```

### View Logs with Timestamps

```bash
docker logs -t <CONTAINER_ID>
```

Use these logs for debugging and monitoring.

---

## **7. Stopping and Removing Containers & Images**

Cleaning up after you're finished helps keep your system efficient and tidy.

### Stop a Running Container

```bash
docker stop <CONTAINER_ID>
```

### Remove a Stopped Container

```bash
docker rm <CONTAINER_ID>
```

### Remove an Image

```bash
docker rmi <IMAGE_ID>
```

---

## **8. System Clean-Up**

Perform regular clean-ups to maintain optimal system performance.

### List All Containers (Including Stopped)

```bash
docker ps -a
```

### List All Images

```bash
docker images
```

This allows you to identify unused or outdated resources and remove them if needed.

---