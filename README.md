# 🐳 Docker Fundamentals — Learning by Doing

## Table of Contents

- [About This Repository](#about-this-repository)
- [Skills & Technologies Demonstrated](#skills--technologies-demonstrated)
- [Code Showcase](#code-showcase)
- [Demos](#demos)
- [Topics Covered](#topics-covered)
- [Future Enhancements](#future-enhancements)
- [Credits](#credits)

---

## About This Repository

This repository demonstrates my practical Docker expertise gained through hands-on learning and real-world application. As a Cloud & NOC Monitoring Engineer with 3+ years of experience, I have transitioned from Hyper-V virtual machines to containerisation, recognising Docker efficiency advantages for modern cloud infrastructure.

This is a hands-on companion to learning Docker based on [Adrian Cantrill Docker course](https://learn.cantrill.io/p/docker-fundamentals).

This repository contains interactive, practical demonstrations of key Docker concepts, and has helped me go from beginner to confident user.

**Real-world applications include:**

- [Docker security with Falcon integration](https://github.com/JThomas404/docker-security-falcon-k8s)
- [Containerised portfolio website with AWS Bedrock AI chatbot](https://github.com/JThomas404/cloudforgex)

---

## Skills & Technologies Demonstrated

**Containerisation & Orchestration:**

- Docker fundamentals, multi-stage builds, Docker Compose
- Container registry management (Docker Hub, AWS ECR)
- Kubernetes (learning path)
- Multi-stage Dockerfile implementations
- Container deployments to Docker Hub and AWS ECR

**Cloud & Infrastructure:**

- AWS services (S3, Lambda, DynamoDB, VPCs)
- Terraform infrastructure as code
- GitHub Actions CI/CD pipelines

**Programming & Scripting:**

- Python (Boto3), Java, Bash scripting
- Linux system administration

---

## Code Showcase

### Docker Compose Configuration

Multi-container WordPress application with MariaDB database:

```yaml
services:
  db:
    image: mariadb:10.6.4-focal
    command: "--default-authentication-plugin=mysql_native_password"
    volumes:
      - mariadb_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
  wordpress:
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - 8081:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
volumes:
  mariadb_data:
  wordpress_data:
```

### Custom Dockerfile

Red Hat UBI8-based web server with custom content:

```dockerfile
FROM redhat/ubi8

LABEL maintainer="Animals4life"

RUN yum -y install httpd

COPY index.html /var/www/html/

COPY containerandcat*.jpg /var/www/html/

ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

EXPOSE 80
```

### Sample Web Application

Simple HTML page demonstrating containerised web content:

```html
<html>
  <head>
    <title>Container of cats</title>
  </head>
  <body>
    <center>
      <h1>IF IT FITS, I SITS (.... in a container.... in a container)</h1>
    </center>

    <section id="photos">
      <img src="containerandcat1.jpg" alt="container and a cat" />
      <img src="containerandcat2.jpg" alt="container and a cat" />
      <img src="containerandcat3.jpg" alt="container and a cat" />
      <img src="containerandcat4.jpg" alt="container and a cat" />
      <img src="containerandcat5.jpg" alt="container and a cat" />
      <img src="containerandcat6.jpg" alt="container and a cat" />
    </section>
  </body>
</html>
```

**Key Technical Implementations:**

- **Multi-container orchestration** with Docker Compose
- **Custom image building** from Red Hat Universal Base Image
- **Volume management** for persistent data storage
- **Environment variable configuration** for database connectivity
- **Port mapping** and service networking

---

## Demos

A growing collection of Docker demonstrations with real commands, screenshots, and results:

| Demo                                                                                          | Description                                                               |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [Working with Existing Docker Images](demos/working-with-existing-docker-images.md)           | Pull, inspect, and run containers using public images from Docker Hub     |
| [Docker Container Volumes](demos/docker-container-volumes.md)                                 | Use bind mounts and volumes to persist container data                     |
| [Docker Container Environment Variables](demos/docker-container-environment-variables.md)     | Pass environment variables to containers at runtime                       |
| [Build a Simple Containerised Application](demos/build-a-simple-containerised-application.md) | Build a Docker image for a basic app using a custom Dockerfile            |
| [Docker Compose](demos/docker-compose.md)                                                     | Define and manage multi-container applications using `docker-compose.yml` |

---

## Topics Covered

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

---

## Future Enhancements

- **Kubernetes:** Container orchestration and cluster management
- **Advanced CI/CD:** Automated Docker deployments with GitHub Actions
- **AWS Integration:** ECS/EKS container services
- **Production Monitoring:** Container observability and logging

---

## Credits

This repository was created whilst following [Adrian Cantrill Docker Fundamentals course](https://learn.cantrill.io/p/docker-fundamentals), with practical implementations and real-world applications added to demonstrate professional Docker competency.

---
