# **Docker Container Environment Variables**

## **Overview**

This guide demonstrates how to deploy Docker containers using environment variables for configuration. We will walk through the setup of a `phpMyAdmin` instance connected to a `MariaDB` container—without using configuration files. Instead, we will use environment variables to provision services, manage credentials, and streamline deployment while avoiding hard-coded values.

You will also explore how container storage works internally and why ephemeral storage is a critical consideration in production environments.

---

## **1. Deploying phpMyAdmin**

[`phpMyAdmin`](https://www.phpmyadmin.net/) is a lightweight, browser-based interface for managing MySQL and MariaDB databases.

We will expose port `80` of the container to `8081` on the Docker host.

The `PMA_ARBITRARY=1` environment variable enables manual server address entry in the user interface.

### **Run:**

```bash
docker run --name phpmyadmin -d \
  -p 8081:80 \
  -e PMA_ARBITRARY=1 \
  phpmyadmin/phpmyadmin
```

This launches a container running the `phpMyAdmin` interface.

![phpMyAdmin Running](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/setup_phpmyadmin.png)

---

## **2. Deploying MariaDB with Environment Variables**

### **Step 1: Pull the MariaDB Image**

```bash
docker pull mariadb:10.6.4-focal
```

### **Step 2: Inspect the Image**

```bash
docker inspect mariadb:10.6.4-focal
```

You will see metadata such as default environment variables, exposed ports, and commands.

---

### **Step 3: Run the MariaDB Container**

```bash
docker run --name db -d \
  -e MYSQL_ROOT_PASSWORD=somewordpress \
  -e MYSQL_DATABASE=wordpress \
  -e MYSQL_USER=wordpress \
  -e MYSQL_PASSWORD=wordpress \
  mariadb:10.6.4-focal \
  --default-authentication-plugin=mysql_native_password
```

#### **Environment Variable Explanation:**

| Variable                        | Purpose                                                  |
|---------------------------------|-----------------------------------------------------------|
| `MYSQL_ROOT_PASSWORD`          | Sets the root account password.                          |
| `MYSQL_DATABASE`               | Creates the `wordpress` database.                        |
| `MYSQL_USER`                   | Creates the `wordpress` user.                            |
| `MYSQL_PASSWORD`               | Sets the password for the `wordpress` user.              |
| `--default-authentication-plugin=mysql_native_password` | Ensures compatibility with phpMyAdmin. |

![Inspect Docker Container](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/docker_inspection.png)

---

## **3. Connecting phpMyAdmin to MariaDB**

### **Step 1: Verify Running Containers**

```bash
docker ps -a
```

Take note of the MariaDB container ID.

### **Step 2: Get the Container’s IP Address**

```bash
docker inspect <MARIADB_ID>
```

Look for the container IP address:

```json
"IPAddress": "172.x.x.x"
```

Use this IP address when connecting from phpMyAdmin.

---

### **Step 3: Access phpMyAdmin**

Visit [http://localhost:8081](http://localhost:8081) in your browser and log in with:

- **Server**: `172.x.x.x` (from the step above)  
- **Username**: `root`  
- **Password**: `somewordpress`

![phpMyAdmin Connected](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/phpmyadmin_container.png)

You should see:
- The `wordpress` database
- The `wordpress` user under **User Accounts**

---

## **4. Exploring Container Storage**

To examine how MariaDB stores data internally:

### **Step into the Container:**

```bash
docker exec -it db bash
```

### **Check Disk Mounts:**

```bash
df -k
```

![Container Storage](https://github.com/JThomas404/docker-course-adrian-cantrill/raw/main/images/container_storage.png)

### **Navigate to MariaDB Data Directory:**

```bash
cd /var/lib/mysql
ls -la
```

This is where the actual database files are stored—inside the container. Since no external volumes are mounted, this storage is **ephemeral** and will be lost if the container is deleted.

---

## **5. Cleanup**

To stop and remove the containers:

```bash
docker stop db phpmyadmin
docker rm db phpmyadmin
```

This deletes the containers, along with all internal data for MariaDB.

---