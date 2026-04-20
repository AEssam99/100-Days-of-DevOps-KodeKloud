# 📦 Day 47: Docker Python App

## 📝 Task

A Python application needs to be containerized and deployed on **App Server 2** with the following requirements:

* A `requirements.txt` file (containing dependencies) is already available at:

  ```
  /python_app/src/requirements.txt
  ```

### Requirements:

1. Create a **Dockerfile** under `/python_app`:

   * Use any Python base image.
   * Install dependencies from `requirements.txt`.
   * Expose port **8086**.
   * Run the application using `server.py`.

2. Build a Docker image named:

   ```
   nautilus/python-app
   ```

3. Run a container named:

   ```
   pythonapp_nautilus
   ```

   * Map container port **8086** to host port **8091**.

---

## ⚙️ Solution

### 1️⃣ Create Dockerfile

Navigate to the project directory:

```bash
cd /python_app
```

Create the Dockerfile:

```bash
vi Dockerfile
```

Add the following content:

```Dockerfile
# Use a lightweight Python base image
FROM python:3.9-slim

# Set working directory inside container
WORKDIR /app

# Copy requirements file into container
COPY src/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code
COPY src/ .

# Expose application port
EXPOSE 8086

# Run the application
CMD ["python", "server.py"]
```

---

### 2️⃣ Build Docker Image

Run the following command:

```bash
docker build -t nautilus/python-app .
```

---

### 3️⃣ Run Docker Container

Create and start the container:

```bash
docker run -d \
  --name pythonapp_nautilus \
  -p 8091:8086 \
  nautilus/python-app
```

---

### 4️⃣ Verification

Check running containers:

```bash
docker ps
```

Test the application:

```bash
curl http://localhost:8091
```

---

## 📌 Explanation

### 🔹 Base Image

```Dockerfile
FROM python:3.9-slim
```

* Uses a lightweight Python image to keep the container small.

### 🔹 Working Directory

```Dockerfile
WORKDIR /app
```

* Sets `/app` as the default directory inside the container.

### 🔹 Dependency Installation

```Dockerfile
COPY src/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
```

* Copies dependencies file and installs required Python packages.

### 🔹 Copy Application Code

```Dockerfile
COPY src/ .
```

* Copies all application files into the container.

### 🔹 Port Exposure

```Dockerfile
EXPOSE 8086
```

* Documents that the app runs on port 8086.

### 🔹 Application Startup

```Dockerfile
CMD ["python", "server.py"]
```

* Starts the Python app when the container runs.

---

## ✅ Final Outcome

* Dockerfile created successfully.
* Image built: `nautilus/python-app`
* Container running: `pythonapp_nautilus`
* Application accessible via:

  ```
  http://localhost:8091
  ```

---

## 🚀 Notes

* `--no-cache-dir` reduces image size by avoiding pip cache.
* Port mapping `8091:8086` allows external access.
* Container runs in detached mode (`-d`).

---
