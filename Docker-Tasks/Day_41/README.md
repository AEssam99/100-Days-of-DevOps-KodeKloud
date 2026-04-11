# 📦 Day 41: Write a Docker File

## 📝 Task

As per the Nautilus application development team requirements, we need to create a custom Docker image on **App Server 1** with the following specifications:

* Use **Ubuntu 24.04** as the base image
* Install **Apache2 web server**
* Configure Apache to run on **port 5003** instead of the default port 80
* Do **not modify any other Apache configurations** (e.g., document root, directories, etc.)
* Create the Dockerfile at:

  ```
  /opt/docker/Dockerfile
  ```

---

## ⚙️ Solution

### 📄 Dockerfile

```dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y apache2 && apt-get clean

RUN sed -i 's/80/5003/g' /etc/apache2/ports.conf && \
    sed -i 's/:80/:5003/g' /etc/apache2/sites-available/000-default.conf

EXPOSE 5003

CMD ["apachectl", "-D", "FOREGROUND"]
```

---

### 🚀 Build & Run

```bash
cd /opt/docker
docker build -t custom-apache .
docker run -d -p 5003:5003 custom-apache
```

---

## 🤔 Why We Use `sed`

We use the `sed` command to **modify Apache configuration files programmatically** without manually editing them.

### Reasons:

* ✅ Keeps default configuration intact (as required)
* ✅ Automates changes during image build
* ✅ Avoids copying and maintaining custom config files
* ✅ Faster and cleaner for small modifications

### What it does:

```bash
sed -i 's/old/new/g' file
```

* `-i` → edit file in place
* `s` → substitute
* `old` → text to replace
* `new` → replacement
* `g` → replace all occurrences

### In this task:

* Change Apache listening port from **80 → 5003**
* Update VirtualHost configuration accordingly

---

## 🧠 Why We Use Foreground Mode

In Docker, containers run a **single main process**. If that process stops, the container stops.

### ❌ Incorrect approach:

```bash
service apache2 start
```

* Runs Apache in background
* Works only during build time
* Container will stop immediately

---

### ✅ Correct approach:

```dockerfile
CMD ["apachectl", "-D", "FOREGROUND"]
```

### Benefits:

* Keeps Apache running in the **foreground**
* Ensures container stays alive
* Follows Docker best practices

---

## 🔑 Key Takeaways

* Docker containers require a **foreground process**
* `sed` allows **safe, automated config updates**
* Only minimal changes were made to meet requirements
* Apache is successfully exposed on **port 5003**

---

## ✅ Final Result

A lightweight, automated Docker image running:

* Ubuntu 24.04
* Apache2
* Listening on port **5003**

Ready for deployment 🚀
