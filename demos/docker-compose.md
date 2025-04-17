# Docker Compose

Docker Compose is a tool for defining and managing multi-container applications. It works using a YAML configuration file called `compose.yaml` or `docker-compose.yml`.

---

## Overview

This project demonstrates the use of Docker Compose to orchestrate a WordPress and MariaDB environment with persistent data storage using **named volumes**. It showcases real-world infrastructure-as-code practices for spinning up services with minimal effort, ensuring scalability and easy data recovery even after containers are removed.

### Key Skills Demonstrated
- Multi-container orchestration with Docker Compose
- Persistent data storage using Docker volumes
- Container networking and environment variable management
- WordPress and MariaDB containerisation
- Hands-on infrastructure automation using YAML

---

## General Workflow

1. Define your application services and dependencies in a `compose.yaml` file.
2. Start your application using the `docker compose` command.
3. Manage services easily (start, stop, rebuild) with Compose commands.

![Docker Compose Overview](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/docker_compose.png)

---

## Example `compose.yaml` File

![YAML Example](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/yaml.png)

```yaml
services:
  db:
    image: mariadb:10.6.4-focal
    command: '--default-authentication-plugin=mysql_native_password'
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

---

## Deploying with Docker Compose

1. Create a file named `compose.yaml` in your current working directory and paste in the content above.
2. Run the following command:

```bash
docker compose up -d
```

- `up` tells Docker Compose to bring up the services defined in the file.
- `-d` runs them in detached mode (in the background).

---

## Setting Up WordPress

1. Visit `http://localhost:8081` in your browser.
2. Complete the WordPress setup wizard:
   - Choose a language
   - Site title: `All the cats`
   - Admin user: `admin`
   - Save the auto-generated password
   - Enter your email
   - Click **Install WordPress**

3. Log in to the admin panel.
4. Go to `Posts` → `Add New`.
5. Create a post, add a gallery with test images, click **Publish**, and then **View Post**.

![WordPress Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/wordpress.png)

---

## Testing Persistence

1. Stop and remove containers:

```bash
docker compose down
```

2. Check that containers are removed:

```bash
docker ps -a
```

3. Check that named volumes still exist:

```bash
docker volume ls
```

> You'll notice the containers are gone, but volumes remain—this means your data is preserved!

4. Bring the services back up:

```bash
docker compose up -d
```

5. Visit `http://localhost:8081` again—your site and post should still be intact!

This is because the database and site files are stored in **named volumes**, which live outside the lifecycle of any individual container.

![Docker Compose Commands](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/compose_commands.png)

---

## Cleanup

- Stop and delete all containers:

```bash
docker compose down
```

- List volumes:

```bash
docker volume ls
```

- Remove the named volumes created by Compose:

```bash
docker volume rm mariadb_data wordpress_data
```

This example demonstrates not just how to use Docker Compose, but **why** you should—by decoupling infrastructure from runtime, you enable repeatability, resilience, and scalable deployments. Perfect for DevOps environments or container-first applications.

---