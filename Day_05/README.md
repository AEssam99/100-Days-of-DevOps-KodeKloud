#Day 1: Linux User Setup with Non-Interactive Shell

##Task

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. 
To initiate testing, the following requirements have been established for <app-server> in the Stratos Datacenter: 
- Install the required SELinux packages. 
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes. 
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight. 
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

##Solution

###Step 1 — Install SELinux Packages
```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils
```

###Step 2 — Permanently Disable SELinux

Edit the SELinux configuration file:
```bash
sudo vi /etc/selinux/config
```

Find this line:
```bash
SELINUX=enforcing
```

OR
```bash
SELINUX=permissive
```

Change it to:
```bash
SELINUX=disabled
```

Save and exit.


