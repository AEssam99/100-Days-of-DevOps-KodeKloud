# Day 44: Write a Docker Compose File

## 📌 Objective

Deploy a containerized Apache HTTPD web server on **App Server 3** using Docker Compose, ensuring proper port and volume mappings for hosting static website content.

---

## 🧾 Requirements

* Docker and Docker Compose must be installed on **App Server 3**
* Directory `/opt/data` must already exist on the host
* Do **not modify** any existing data inside `/opt/data`

---

## 📁 File Location

Create the Docker Compose file at:

```
/opt/docker/docker-compose.yml
```

---

## ⚙️ Docker Compose Configuration

Create and edit the file:

```
vi /opt/docker/docker-compose.yml
```

Add the following configuration:

```yaml
version: '3.8'

services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8085:80"
    volumes:
      - /opt/data:/usr/local/apache2/htdocs
```

---

## 🚀 Deployment Steps

1. Navigate to the Docker directory:

   ```bash
   cd /opt/docker
   ```

2. Start the container:

   ```bash
   docker-compose up -d
   ```

---

## 🔍 Verification

### Check running containers:

```bash
docker ps
```

Ensure you see a container named `httpd`.

---

### Test Web Access:

Open a browser or use curl:

```bash
curl http://<App_Server_3_IP>:8085
```

You should see the hosted static content from `/opt/data`.

---

## ⚠️ Notes

* Container name must be **httpd**
* File name must be exactly:

  ```
  /opt/docker/docker-compose.yml
  ```
* Do not change or delete any existing files inside:

  ```
  /opt/data
  ```

---

## ✅ Outcome

* Apache HTTPD container successfully deployed
* Port `8085` on host mapped to `80` in container
* Static content served from `/opt/data`

---
