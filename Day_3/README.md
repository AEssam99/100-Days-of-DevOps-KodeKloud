# Day 3: Secure Root SSH Access

## Task

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.



Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.


## Solution

### 1) Edit SSH configuration
```bash
sudo vi /etc/ssh/sshd_config
```
Find this line:
```bash
PermitRootLogin yes
```

Change it to:
```bash
PermitRootLogin no
```

If the line is commented (#), uncomment and set to no.

### 2) Save and exit (in vi)
```ruby
:wq
```
### 3) Restart SSH service

```bash
sudo systemctl restart sshd
```

