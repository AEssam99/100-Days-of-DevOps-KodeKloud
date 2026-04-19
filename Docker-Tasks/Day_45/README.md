# Day 45: Resolve Dockerfile Issues
## 📋 Task Description

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a Dockerfile on App Server 2 in Stratos DC. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:

* The Dockerfile is placed on App Server 2 under `/opt/docker` directory.
* Fix the issues with this file and make sure it is able to build the image.
* Do not change base image, any other valid configuration within Dockerfile, or any of the data being used (e.g., `index.html`).

**Note:** Once you click on FINISH, all existing containers will be destroyed and a new image will be built from your Dockerfile.

---

## 📌 Overview

The Dockerfile fails during the build process due to incorrect configuration file paths. The issue stems from referencing a non-existent directory (`conf.d`) in the official Apache `httpd` Docker image.

---

## ❌ Problem

The original Dockerfile attempts to modify:

```
/usr/local/apache2/conf.d/httpd.conf
```

However, in the official `httpd:2.4.43` image:

* The `conf.d` directory does **not exist by default**
* Apache uses:

```
/usr/local/apache2/conf/httpd.conf
```

### Errors caused:

* `sed: can't read conf.d/httpd.conf: No such file or directory`
* Docker build fails

---

## ✅ Solution

Update all configuration modification commands to target the correct file:

```
/usr/local/apache2/conf/httpd.conf
```

This ensures:

* Apache configuration is properly modified
* Docker image builds successfully
* No changes to base image or data (as required)

---

## 🛠️ Fixed Dockerfile

```dockerfile
FROM httpd:2.4.43

# Change Apache to listen on port 8080 instead of 80
RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

# Enable SSL module
RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Enable socache_shmcb module (required for SSL)
RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Enable SSL configuration include
RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Copy SSL certificates
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key

# Copy website content
COPY html/index.html /usr/local/apache2/htdocs/
```

---

## 🔍 Key Changes

| Issue                             | Fix                         |
| --------------------------------- | --------------------------- |
| Invalid path: `conf.d/httpd.conf` | Replaced with correct path  |
| Relative paths                    | Converted to absolute paths |
| Non-existent config file          | Use main Apache config      |

---

## 🧪 Build the Image

Run the following command on **App Server 2**:

```bash
docker build -t httpd-fixed /opt/docker
```

---

## 🚀 Expected Outcome

* Docker image builds successfully ✅
* Apache listens on port **8080** ✅
* SSL modules are enabled ✅
* Website content is served correctly ✅

---

## 💡 Notes

* The official Apache `httpd` image uses a **single main configuration file**
* `conf.d` is **not available unless manually configured**
* Minimal changes were applied to comply with task constraints

---

## 🏁 Conclusion

The issue was caused by incorrect assumptions about Apache configuration structure. By correcting the file paths to use the default `httpd.conf`, the Docker build process works as expected without violating any task requirements.
