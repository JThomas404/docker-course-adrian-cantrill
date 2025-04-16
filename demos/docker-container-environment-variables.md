## docker-container-environment-variables.md

### Overview

This documentation demonstrates how to deploy Docker containers using environment variables. I walked through setting up a phpMyAdmin instance to manage a MariaDB container, both launched without manual configuration files—only environment variables. It showcases how environment variables can be used to provision services, manage credentials, and streamline deployment without hard-coding configurations.

The walkthrough also highlights how container storage works internally and why ephemeral storage is a consideration in production.

---

### 1. Setting Up `phpMyAdmin`

`phpMyAdmin` is a lightweight, browser-based database management tool for MySQL and MariaDB. We'll launch it and expose port 80 of the container to port 8081 on the Docker host.

The `-e PMA_ARBITRARY=1` environment variable allows users to manually enter the server address in the UI.

#### Run:

```bash
docker run --name phpmyadmin -d -p 8081:80 -e PMA_ARBITRARY=1 phpmyadmin/phpmyadmin
```

This creates a container running the `phpmyadmin` database management app.

![phpMyAdmin Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/setup_phpmyadmin.png)

---

### 2. Creating a MariaDB Container with Environment Variables

#### Step 1: Pull the MariaDB Image

```bash
docker pull mariadb:10.6.4-focal
```

#### Step 2: Inspect the Image Metadata

```bash
docker inspect mariadb:10.6.4-focal
```

This provides image metadata, including environment variables, default command, and exposed ports.

---

#### Step 3: Run the MariaDB Container Using Environment Variables

```bash
docker run --name db \
  -e MYSQL_ROOT_PASSWORD=somewordpress \
  -e MYSQL_PASSWORD=wordpress \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wordpress \
  -d mariadb:10.6.4-focal \
  --default-authentication-plugin=mysql_native_password
```

##### Explanation of Environment Variables:

- `MYSQL_ROOT_PASSWORD` – Sets the root user password.  
- `MYSQL_USER` – Creates a new user.  
- `MYSQL_PASSWORD` – Sets that user's password.  
- `MYSQL_DATABASE` – Creates a new database with that name.  
- `--default-authentication-plugin=mysql_native_password` – Specifies the authentication plugin.

![Inspect Docker Container](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/docker_inspection.png)

---

### 3. Connecting `phpMyAdmin` to the MariaDB Container

#### Step 1: Verify the Container is Running

```bash
docker ps -a
```

Take note of the MariaDB container’s ID (let’s refer to it as `MARIADB_ID`).

#### Step 2: Inspect the Container

```bash
docker inspect MARIADB_ID
```

Look for the following in the output:

```json
"IPAddress": "172.x.x.x"
```

Take note of this IP as `DB_IP`.

#### Step 3: Open phpMyAdmin in Your Browser

Navigate to [http://localhost:8081](http://localhost:8081) and log in using:

- **Server**: `DB_IP`  
- **Username**: `root`  
- **Password**: `somewordpress`

![phpMyAdmin Connected](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/phpmyadmin_container.png)

- Confirm that the `wordpress` user exists under **User Accounts**  
- Verify that the `wordpress` database appears in the left sidebar

---

### 4. Exploring Container Storage

Access the MariaDB container shell to inspect where data is stored:

```bash
docker exec -it db bash
```

Once inside:

```bash
df -k
```

![Container Storage](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/container_storage.png)

This shows storage is local to the container. If the container is removed, its data is lost.

MariaDB stores its data at `/var/lib/mysql`. To inspect:

```bash
cd /var/lib/mysql
ls -la
```

This is the actual on-container data for the `wordpress` database. No external volumes are attached.

---

### 5. Cleanup

To remove the containers and clean up:

```bash
docker stop db
docker rm db

docker stop phpmyadmin
docker rm phpmyadmin
```

These commands remove both containers entirely. In the next demo, we’ll introduce persistent storage using Docker volumes.

---