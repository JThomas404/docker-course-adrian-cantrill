# Docker Container Volumes

## Overview

This project demonstrates the use of **Docker volumes** and **bind mounts** to manage persistent storage in containerised environments. The focus is on ensuring that data—specifically in MariaDB containers—remains persistent across container restarts and removals.

### Key Highlights

- **Docker storage expertise**: Showcases best practices in leveraging volumes and bind mounts for reliable data persistence.
- **Database container management**: Practical setup of MariaDB containers integrated with phpMyAdmin for web-based database administration.
- **Command-line proficiency**: Utilises essential Docker commands (`docker run`, `docker volume create`, etc.) for container and volume management.
- **Persistence assurance**: Ensures data durability beyond the lifecycle of containers, a critical consideration for production-grade environments.

---

## Prerequisites

Before proceeding, ensure you’ve completed the previous demo on environment variables and understand its key components.

Start by recreating the `phpmyadmin` container using:

```bash
docker run --name phpmyadmin -d -p 8081:80 -e PMA_ARBITRARY=1 phpmyadmin/phpmyadmin
```

![phpMyAdmin Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/setup_phpmyadmin.png)

---

## Bind Mounts

Bind mounts map a host file or directory into a container, allowing shared access between them. This is useful for debugging or syncing data between containers and the host. In this section, we bind the MariaDB data directory from the container to a directory on the host.

### Step 1: Create the Host Directory

Choose a working directory:

- **Windows**:  
  ```cmd
  cd %HOMEDRIVE%
  ```
- **macOS/Linux**:  
  ```bash
  cd ~
  ```

Then create the folder:

```bash
mkdir mariadb_data
```

### Step 2: Start the MariaDB Container with a Bind Mount

The MariaDB data directory is located at `/var/lib/mysql` within the container. We will bind this to the `mariadb_data` folder created above.

#### macOS/Linux  
*Choose either syntax; both are equivalent:*

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 --mount type=bind,source="$(pwd)"/mariadb_data,target=/var/lib/mysql \
 -d mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 -v "$(pwd)"/mariadb_data:/var/lib/mysql \
 -d mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

#### Windows  
*Choose either syntax (note: bind mounts can be tricky on Windows):*

```cmd
docker run --name db -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress --mount type=bind,source=%cd%/mariadb_data,target=/var/lib/mysql -d mariadb:10.6.4-focal --default-authentication-plugin=mysql_native_password
```

```cmd
docker run --name db -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -v %cd%/mariadb_data:/var/lib/mysql -d mariadb:10.6.4-focal --default-authentication-plugin=mysql_native_password
```

After launching the container, navigate into the `mariadb_data` folder. You’ll see all the files from the MariaDB container’s `/var/lib/mysql` directory reflected here. Changes on either side (host or container) will mirror each other.

![Bind Mounts](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/bind_mounts.png)

---

## Docker Volumes

Volumes are the **preferred** method for persistent storage in Docker. They are fully managed by Docker, decoupled from the host filesystem, and are reliable across platforms—including Windows.

### Step 1: Create a Docker Volume

```bash
docker volume create mariadb_data
```

Verify the volume exists:

```bash
docker volume ls
```

Inspect volume metadata:

```bash
docker volume inspect mariadb_data
```

To remove a volume:

```bash
docker volume rm mariadb_data
```

---

### Step 2: Run a Container Using a Volume

*Choose either syntax:*

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 --mount source=mariadb_data,target=/var/lib/mysql \
 -d mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 -v mariadb_data:/var/lib/mysql \
 -d mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

You’ll see the volume automatically created via:

```bash
docker volume ls
```

Now stop and remove the container:

```bash
docker stop db
docker rm db
```

The volume remains intact:

```bash
docker volume ls
```

You can reuse it by launching a new container with the same volume:

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 -v mariadb_data:/var/lib/mysql \
 -d mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

Retrieve the container's internal IP:

```bash
docker inspect db
```

Then access phpMyAdmin at [http://localhost:8081](http://localhost:8081) using:
- **Server**: internal container IP
- **Username**: `root`
- **Password**: `somewordpress`

You’ll see that all data remains intact from the previous session.

![MariaDB Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/mariadb.png)

---

## Cleanup

To stop and remove containers and the volume:

```bash
docker stop db
docker rm db
docker stop phpmyadmin
docker rm phpmyadmin
docker volume rm mariadb_data
```

![Manual Volume Remove](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/manual_volume_rm.png)

**Note**: Volumes must be explicitly deleted—they persist beyond the container's lifecycle unless manually removed.

---