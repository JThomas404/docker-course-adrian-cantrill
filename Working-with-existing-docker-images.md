# Working with Existing Docker Images

## Verifying What’s Currently on the Docker Host

```bash
docker ps
docker images
```

---

## Getting an Existing Image from Docker Hub

1. Visit [hub.docker.com](https://hub.docker.com/)
2. Search for `containerofcats`
3. Click on `acantril/containerofcats`

Pull the image:

```bash
docker pull acantril/containerofcats
```

![Pulled Docker Image](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/pulled_docker_image.png)

*You’ll notice it pulls several filesystem layers...*

---

## Working with the Docker Image & Containers Locally

List all images:

```bash
docker images -a
```

Inspect an image:

```bash
docker inspect <IMAGE_ID>
```

![Docker Inspect](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/docker_inspect.png)

---

### Run a Container from the Image

```bash
docker run -p 8081:80 acantril/containerofcats
```

> Open your browser and navigate to `http://localhost:8081` — see the cats?  
> Note: The terminal is now attached to the container. Press `Ctrl+C` to stop it — this will also stop the container.

![Port Mapping](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/port_mapping.png)

Check container status:

```bash
docker ps
```

Confirmation that we can now access the website on port 8081:

![Container of Cats](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/container_of_cats.png)

---

### Run in Detached Mode

```bash
docker run -p 8081:80 -d acantril/containerofcats
```

![Detached Mode](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/detached_mode.png)

> This runs the container in the background, and your terminal will remain free.

Show port mappings:

```bash
docker port <CONTAINER_ID>
```

---

## Interacting with Running Containers

Run a command inside the container (e.g., list processes):

```bash
docker exec -it <CONTAINER_ID> ps -aux
```

Start a shell inside the container:

```bash
docker exec -it <CONTAINER_ID> sh
```

Restart the container:

```bash
docker restart <CONTAINER_ID>
```

Stop the container:

```bash
docker stop <CONTAINER_ID>
```

List running and all containers:

```bash
docker ps
docker ps -a
```

Start a stopped container:

```bash
docker start <CONTAINER_ID>
```

![Interacting with Containers](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/interacting_with_containers.png)

---

## Viewing Container Logs

Show logs:

```bash
docker logs <CONTAINER_ID>
```

Show logs with timestamps:

```bash
docker logs <CONTAINER_ID> -t
```

---

## Clean-Up Steps from This Demo

Stop the running container:

```bash
docker stop <CONTAINER_ID>
```

Remove the container:

```bash
docker rm <CONTAINER_ID>
```

List local images:

```bash
docker images
```

Remove the pulled image:

```bash
docker rmi <IMAGE_ID>
```

---

**PS**: This documentation was adapted from Adrian Cantrill's Docker Fundamentals course repository.

---