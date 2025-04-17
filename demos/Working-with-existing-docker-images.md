## working with Docker Images

This guide demonstrates how to pull and manage Docker images, run containers, and interact with them, following best practices for clarity and efficiency. It's designed to be easily understood by both developers and hiring managers, providing clear steps for anyone unfamiliar with Docker.

---

### 1. Verifying What’s Currently on the Docker Host

Before interacting with Docker containers, it’s important to know what is already running on your system. These commands allow you to inspect the status of Docker containers and images.

#### List Running Containers

```bash
docker ps
```

This command will show all running containers. It’s essential for identifying which containers are currently active on your Docker host.

#### List All Docker Images

```bash
docker images
```

This command lists all available images, including those that are not actively running. It's helpful for identifying which images can be used to create new containers.

---

### 2. Pulling an Existing Image from Docker Hub

Docker Hub is the default public repository for Docker images. To use an image from Docker Hub, follow these steps:

#### Step 1: Visit Docker Hub

- Go to [hub.docker.com](https://hub.docker.com/)
- Search for the image you need (e.g., `containerofcats`)

#### Step 2: Pull the Image

After selecting the desired image, use the following command to pull it to your local machine:

```bash
docker pull acantril/containerofcats
```

This command downloads the image `acantril/containerofcats` from Docker Hub to your local machine.

![Image Pull](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/pulled_docker_image.png)

---

### 3. Running a Container from the Image

Once you have the image locally, you can start a container based on it.

#### Run a Container with Port Mapping

```bash
docker run -p 8081:80 acantril/containerofcats
```

This command runs a container from the `acantril/containerofcats` image and maps port 80 in the container to port 8081 on your local machine. You can now access the application in your browser at `http://localhost:8081`.

![Container Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/container_of_cats.png)

---

### 4. Running Containers in Detached Mode

Running containers in detached mode allows them to run in the background, freeing up the terminal for other tasks.

#### Start a Container in Detached Mode

```bash
docker run -p 8081:80 -d acantril/containerofcats
```

The `-d` flag runs the container in detached mode.

#### Verify Container Status

To confirm that your container is running in detached mode, use:

```bash
docker ps
```

This command will show you all running containers, including those in detached mode.

---

### 5. Interacting with Running Containers

Docker containers are not just passive entities; you can interact with them to perform tasks, troubleshoot, or inspect logs.

#### Execute a Command Inside the Container

To execute a command inside a running container (for example, listing running processes):

```bash
docker exec -it <CONTAINER_ID> ps -aux
```

#### Open a Shell Inside the Container

For more direct interaction, you can start an interactive shell inside the container:

```bash
docker exec -it <CONTAINER_ID> sh
```

This allows you to explore the container’s file system, install additional software, or run commands interactively.

---

### 6. Viewing Container Logs

Logs are essential for troubleshooting and monitoring containers. Use the following command to view logs from a running container:

```bash
docker logs <CONTAINER_ID>
```

For more detailed logs, including timestamps, use:

```bash
docker logs <CONTAINER_ID> -t
```

---

### 7. Stopping, Removing, and Cleaning Up Containers

After finishing with a container, it's good practice to stop and clean it up to free up system resources.

#### Stop a Running Container

```bash
docker stop <CONTAINER_ID>
```

#### Remove a Stopped Container

```bash
docker rm <CONTAINER_ID>
```

#### Remove an Image

If you no longer need an image, you can remove it as well:

```bash
docker rmi <IMAGE_ID>
```

---

### 8. Final Clean-Up

After working with Docker, ensure that your system is clean by removing any unused containers or images that are taking up space.

- **List all containers (including stopped ones):**

```bash
docker ps -a
```

- **List all images:**

```bash
docker images
```

---