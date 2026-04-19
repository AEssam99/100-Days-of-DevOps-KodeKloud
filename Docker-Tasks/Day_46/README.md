# 📦 Day 46: Deploy an App on Docker Containers

## 🧾 Task Description

The Nautilus Application Development team has completed building an application that needs to be deployed on a containerized platform. Before going live, the DevOps team must test the deployment on **App Server 2 in Stratos Datacenter** by setting up a complete containerized stack using a **Docker Compose file**.

---

## 🎯 Requirements

Create a Docker Compose file with the following specifications:

### 📁 File Location

* `/opt/devops/docker-compose.yml` *(must be named exactly as specified)*

---

## 🧩 Services Configuration

### 🌐 Web Service (PHP)

* **Container Name:** `php_web`
* **Image:** `php:<any-apache-tag>` (e.g., `php:8.2-apache`)
* **Port Mapping:**

  * Host: `5000` → Container: `80`
* **Volume Mapping:**

  * Host: `/var/www/html` → Container: `/var/www/html`

---

### 🗄️ Database Service (MariaDB)

* **Container Name:** `mysql_web`
* **Image:** `mariadb:latest` (or any valid tag)
* **Port Mapping:**

  * Host: `3306` → Container: `3306`
* **Volume Mapping:**

  * Host: `/var/lib/mysql` → Container: `/var/lib/mysql`
* **Environment Variables:**

  * `MYSQL_DATABASE=database_web`
  * `MYSQL_USER=<custom_user>` *(must NOT be root)*
  * `MYSQL_PASSWORD=<complex_password>`
  * `MYSQL_ROOT_PASSWORD=<root_password>`

---

## 🛠️ Solution

### 1. Create Directory (if not exists)

```bash
sudo mkdir -p /opt/devops
```

---

### 2. Create Docker Compose File

```bash
sudo vi /opt/devops/docker-compose.yml
```

---

### 3. Add the Following Configuration

```yaml
version: '3.8'

services:
  web:
    image: php:8.2-apache
    container_name: php_web
    ports:
      - "5000:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    image: mariadb:latest
    container_name: mysql_web
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_USER: devuser
      MYSQL_PASSWORD: DevUser@123
      MYSQL_ROOT_PASSWORD: Root@123
```

---

### 4. Start the Services

```bash
cd /opt/devops
sudo docker-compose up -d
```

> ⚠️ If using newer Docker versions:

```bash
sudo docker compose up -d
```

---

### 5. Verify Containers

```bash
docker ps
```

Ensure both containers are running:

* `php_web`
* `mysql_web`

---

### 6. Test the Application

Use curl to verify access:

```bash
curl http://<server-ip>:5000/
```

---

## ✅ Expected Outcome

* PHP Apache container is accessible via port **5000**
* MariaDB container is running and initialized with:

  * Database: `database_web`
  * Custom user credentials
* Volumes ensure data persistence
* Full stack is successfully deployed using Docker Compose

---

## 🧠 Notes

* Ensure `/var/www/html` contains your application files.
* Ensure `/var/lib/mysql` has proper permissions.
* Avoid using `root` as `MYSQL_USER` (as per requirement).
* If port 3306 is already in use, stop conflicting services before deployment.

---

## 🚀 You're Good to Go!

This setup simulates a production-like environment for testing the Nautilus application before going live.
