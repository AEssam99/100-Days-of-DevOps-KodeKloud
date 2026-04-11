# 📦 Day 40: Docker EXEC Operations

## 🧾 Task Overview

Complete the pending configuration on the **kkloud container** running on **App Server 1** in Stratos Datacenter with the following requirements:

* Install **Apache2**
* Configure Apache to listen on **port 8082**
* Ensure Apache is **running**
* Keep the **container in running state**

---

## 🛠️ Step 1: Access the Container

```bash
docker exec -it kkloud bash
```

---

## 📥 Step 2: Install Apache2

```bash
apt update
apt install -y apache2
```

---

## ⚙️ Step 3: Configure Apache to Use Port 8082

### Update ports configuration:

```bash
vi /etc/apache2/ports.conf
```

Change:

```apache
Listen 80
```

To:

```apache
Listen 8082
```

---

### Update Virtual Host configuration:

```bash
vi /etc/apache2/sites-available/000-default.conf
```

Change:

```apache
<VirtualHost *:80>
```

To:

```apache
<VirtualHost *:8082>
```

✔ This ensures Apache listens on **all interfaces (0.0.0.0)**

---

## ▶️ Step 4: Start Apache Service

```bash
service apache2 start
```

---

## 🔍 Step 5: Verify Apache Status

```bash
service apache2 status
```

Expected output:

```
apache2 is running
```

---

## 🌐 Step 6: Verify Listening Port

```bash
netstat -tulnp | grep 8082
```

Expected output:

```
0.0.0.0:8082 LISTEN
```

---

## 🧪 Step 7: Test Apache

```bash
curl http://localhost:8082
```

✔ Should return Apache default HTML page

---

## ⚠️ Notes

* The warning:

  ```
  Could not reliably determine the server's fully qualified domain name
  ```

  is harmless and does not affect functionality.

* Optional fix:

  ```bash
  echo "ServerName localhost" >> /etc/apache2/apache2.conf
  ```

* `service apache2 enable` is **not supported** in containers because they do not use systemd.

---

## ✅ Final Validation Checklist

* ✔ Apache installed successfully
* ✔ Port changed to **8082**
* ✔ Listening on all interfaces (`0.0.0.0`)
* ✔ Apache service is running
* ✔ Container remains in running state

---

## 🎯 Conclusion

The Apache service has been successfully configured and is running on **port 8082** inside the **kkloud container**, fulfilling all task requirements.
