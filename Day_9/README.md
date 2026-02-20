# Day 9: MariaDB Troubleshooting

## Task

There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server
Look into the issue and fix the same.

## Solution

### 1) Check the service status and logs
```bash
# To check the status of the service (mariadb in our problem statement):
sudo systemctl status mariadb

# To check the service logs:
sudo cat /var/log/mariadb/mariadb.log
```

Result: 
The MariaDB service is in inactive state and from the logs, we can tell that the service stopped with a clean exit which means, that it did not crash. Something else stopped it from running.

### 2) Check journalctl logs
```bash
# To inspect the system journal logs:
sudo journalctl -u mariadb 

# Look for lines like:
# systemd[1]: Stopping MariaDB database server...
# systemd[1]: mariadb.service: Succeeded.
# If you see that, it confirms that systemd stopped it normally which means that maybe a manual stop or a script executed it.

```

### 3) Restart the Service
```bash
sudo systemctl restart mariadb
```

Result:
The service failed with an exit code 1.
So we have to double check the logs again.

### 4) Permission Error

The logs state that mariadb tries to start but fails at the final step. This means MariaDB doesn’t have permission to write its PID file to /run/mariadb/, so the service aborts startup. We need to check the permission of the /run/mariadb directory.

```bash 
ls -ld /run/mariadb
```

Result:
```bash
drwxr-xr-x 2 root mysql 40 nov ....
```
Which means there is an error with the owner as it must be mysql not root.

### 5) Solve the issue
```bash
sudo chown mysql:mysql /run/mariadb
```

### 6) Restart the Mariadb again
```bash
sudo systemctl restart mariadb
```
Result:
It is working normally now.


