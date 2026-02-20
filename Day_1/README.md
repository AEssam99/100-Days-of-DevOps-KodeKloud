# Day 1: Linux User Setup with Non-Interactive Shell

##Task

Create a user on apache with non interactive shell

##Solution

###1) Create a non-login Apache user
```bash
sudo useradd -r -s /sbin/nologin -d /var/www -c "Apache Service User" apache
```

###2) If user already exists → modify shell
```bash
sudo usermod -s /sbin/nologin apache
```

###3) Verify user settings
```bash
grep apache /etc/passwd
```
