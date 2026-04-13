# 📦 Day 42: Create a Docker Network

## 📝 Task

The Nautilus DevOps team required the creation of a custom Docker network for future application deployments on **App Server 2 in Stratos DC**.

### Requirements:

* Create a Docker network named **`beta`**
* Use **bridge driver**
* Configure:

  * Subnet: `10.10.1.0/24`
  * IP Range: `10.10.1.0/24`

---

## 🛠️ Solution

### 🔹 Step 1: Connect to App Server 2

```bash
ssh user@app-server-2
```

---

### 🔹 Step 2: Ensure Docker Service is Running

```bash
docker ps
```

If Docker is not running:

```bash
sudo systemctl start docker
```

---

### 🔹 Step 3: Create the Docker Network

```bash
docker network create \
--driver bridge \
--subnet 10.10.1.0/24 \
--ip-range 10.10.1.0/24 \
beta
```

---

### 🔹 Step 4: Verify Network Creation

List all networks:

```bash
docker network ls
```

Inspect the created network:

```bash
docker network inspect beta
```

---

## 📖 Explanation

### 🔸 Why Bridge Driver?

* The **bridge driver** is the default Docker network type.
* It allows containers on the same host to communicate with each other.
* Suitable for single-host environments like this use case.

---

### 🔸 Subnet vs IP Range

| Parameter    | Purpose                                          |
| ------------ | ------------------------------------------------ |
| `--subnet`   | Defines the full network range                   |
| `--ip-range` | Restricts the pool of IPs assigned to containers |

In this task, both are set to `10.10.1.0/24`, meaning:

* All IPs in this range are available for container assignment.

---

## ✅ Expected Outcome

After successful execution:

```bash
docker network ls
```

Output should include:

```
NETWORK ID     NAME    DRIVER    SCOPE
xxxxxx         beta    bridge    local
```

---

## 💡 Notes / Best Practices

* Use **custom networks** instead of the default `bridge` for better isolation.
* For multi-host communication, consider using **overlay networks**.
* Always verify configurations using `docker network inspect`.

---

## 🎯 Conclusion

The `beta` Docker network has been successfully created with the required configuration using the bridge driver, ensuring proper IP management and container communication for future deployments.

---
