# 📦 Day 43: Docker Ports Mapping
## 📝 Task

The Nautilus DevOps team needs to deploy an NGINX-based container on **Application Server 1** in Stratos Datacenter with the following requirements:

* Pull the `nginx:stable` Docker image
* Create a container named **news**
* Map **host port 6000** to **container port 80**
* Ensure the container remains in a running state

---

## ⚙️ Solution

### 1. Pull the NGINX Image

```bash
docker pull nginx:stable
```

---

### 2. Run the Container

```bash
docker run -d --name news -p 6000:80 nginx:stable
```

---

## 🔍 Verification

### Check Running Containers

```bash
docker ps
```

Expected output should include:

* Container name: `news`
* Port mapping: `0.0.0.0:6000->80/tcp`

---

### Test the Application

#### Using curl:

```bash
curl http://localhost:6000
```

#### Using browser:

```
http://<Application-Server-1-IP>:6000
```

You should see the default **NGINX welcome page**.

---

## 💡 Explanation

* `docker pull nginx:stable`
  Downloads the official stable NGINX image from Docker Hub.

* `docker run`
  Creates and starts a new container.

* `-d` (detached mode)
  Runs the container in the background.

* `--name news`
  Assigns a custom name to the container.

* `-p 6000:80`
  Maps:

  * Host port **6000**
  * Container port **80** (default for NGINX)

---

## ✅ Result

An NGINX container named **news** is successfully running and accessible via:

```
http://<Application-Server-1-IP>:6000
```

---
