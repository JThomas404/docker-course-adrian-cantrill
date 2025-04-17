Here's the corrected and structured version of the headings and subheadings in your document:

---

## Docker Container Volumes

### Overview

This project demonstrates the use of **Docker volumes** and **bind mounts** for managing persistent storage in containerised environments. It focuses on ensuring that data in containers, specifically MariaDB, remains intact across container lifecycles.

Key highlights:

- **Expertise in Docker storage solutions**: Demonstrates effective use of bind mounts and volumes for data persistence in MariaDB containers.
- **Containerised database management**: Practical experience setting up MariaDB containers and integrating with phpMyAdmin for database management.
- **Proficiency with Docker commands**: Familiarity with essential Docker commands (`docker run`, `docker volume create`, etc.) and volume management.
- **Handling data persistence**: Ensures data survives container restarts and removals, crucial for production environments.

---

### Prerequisite

Make sure you have completed the previous demo lesson on environment variables and understand all parts of that demo.  
Recreate the `phpmyadmin` container via the command below:

```bash
docker run --name phpmyadmin -d -p 8081:80 -e PMA_ARBITRARY=1 phpmyadmin/phpmyadmin
```

![phpMyAdmin Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/setup_phpmyadmin.png)

### Bind Mounts

Bind mounts allow us to mount a file or directory on the host machine into a container. This has pros and cons, but it can be useful if you need to see files on the container host or access a shared collection of files between containers or other compute services.

In this example, we're going to create a bind mount for the MariaDB data folder in the container and map it to a folder on the Docker host.

We can do this in two ways, using the `-v` option or the `--mount` option. They both do the same thing, but they look different.

As a reminder, in the environment variables demo lesson, we created a MariaDB container using this command:

```bash
docker run --name db -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -d mariadb:10.6.4-focal --default-authentication-plugin=mysql_native_password
```

And the MariaDB data was stored in the container in the `/var/lib/mysql` folder.

First, on your local machine/Docker host, we will create a folder for the MariaDB data, and we will call it `mariadb_data`.

- Move into a folder where you are OK creating files and folders, either your home folder or a temp folder.  
- I'll assume your home folder.
- **Windows**: `cd %HOMEDRIVE%`  
- **macOS/Linux**: `cd ~`  
- Create a folder called `mariadb_data`  
- `mkdir mariadb_data`

#### macOS / Linux  
*Both of these do the same thing, pick one*

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 --mount type=bind,source="$(pwd)"/mariadb_data,target=/var/lib/mysql \
 -d \
 mariadb:10.6.4-focal \
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
 -d \
 mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

#### Windows  
*Both of these do the same thing, pick one*  
*Bind mounts often require specific configuration on Windows Docker hosts. It's often tricky to get it working.*

```bash
docker run --name db -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress --mount type=bind,source=%cd%/mariadb_data,target=/var/lib/mysql -d mariadb:10.6.4-focal --default-authentication-plugin=mysql_native_password
```

```bash
docker run --name db -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_PASSWORD=wordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -v %cd%/mariadb_data:/var/lib/mysql -d mariadb:10.6.4-focal --default-authentication-plugin=mysql_native_password
```

#### macOS / Linux & Windows

In all of the above cases, move into the `mariadb_data` folder and do a listing.  
You should be able to see all the files and folders created by the bind mount into the container.  
Any data stored in the `/var/lib/mysql` folder in the container is stored in this folder on the host.  
Any changes made to the `/var/lib/mysql` in the container will be reflected in this folder.  
Any changes made to this folder will be reflected in the `/var/lib/mysql` folder in the container.  

![Bind Mounts](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/bind_mounts.png)

### Volumes

Volumes are the preferred way to add storage to Docker containers outside of the lifecycle of a container.  
They are managed entirely by Docker and work flawlessly on Windows container hosts.

To create a volume, run:

```bash
docker volume create mariadb_data
```

You can list any volumes via:

```bash
docker volume ls
```

You can review full metadata for a volume via:

```bash
docker volume inspect mariadb_data
```

You can delete a volume via:

```bash
docker volume rm mariadb_data
```

And verify it's been removed via:

```bash
docker volume ls
```

If you run a container and specify a volume that doesn't already exist, Docker will create it for you.

*Both of these do the same thing, pick one*

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 --mount source=mariadb_data,target=/var/lib/mysql \
 -d \
 mariadb:10.6.4-focal \
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
 -d \
 mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

Notice via `docker volume ls` how this has automatically created a volume.  
If we stop the container with:

```bash
docker stop db
```

And then delete it with:

```bash
docker rm db
```

Notice how the volume still exists:

```bash
docker volume ls
```

![Volumes](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/volumes.png)

If we start up the container again, we can use the same volume and any data stored within it persists — so data can live on beyond the lifecycle of the container.

```bash
docker run \
 --name db \
 -e MYSQL_ROOT_PASSWORD=somewordpress \
 -e MYSQL_PASSWORD=wordpress \
 -e MYSQL_DATABASE=wordpress \
 -e MYSQL_USER=wordpress \
 -v mariadb_data:/var/lib/mysql \
 -d \
 mariadb:10.6.4-focal \
 --default-authentication-plugin=mysql_native_password
```

Get the internal container IP via:

```bash
docker inspect db
```

And then browse to [http://localhost:8081](http://localhost:8081) and log in with that IP, `root`, and `somewordpress`.  
Notice how it works fine?

![MariaDB Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/mariadb.png)

### Cleanup

```bash
docker stop db
docker rm db
docker stop phpmyadmin
docker rm phpmyadmin
docker volume rm mariadb_data
```

![Manual Volume Remove](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/manual_volume_rm.png)

Note that we need to manually remove the volume as it persistently remains even after removing the container.

---