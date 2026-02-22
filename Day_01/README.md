# Day 1: Linux User Setup with Non-Interactive Shell

## Task

To accommodate the backup agent tool’s specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here’s your task:

Create a user named john with a non-interactive shell on App Server 2.

## Solution

### 1) Create a non-login Apache user
```bash
sudo useradd -r -s /sbin/nologin -d /var/www -c "Apache Service User" apache
```

### 2) If user already exists → modify shell
```bash
sudo usermod -s /sbin/nologin apache
```

### 3) Verify user settings
```bash
grep apache /etc/passwd
```
