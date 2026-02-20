# Day 7: Linux SSH Authentication

## Task

The system admins team of xFusionCorp Industries has set up scripts on the jump host that run at regular intervals and perform operations on all app servers in the Stratos Datacenter. 
To make these scripts work properly we need to ensure the thor user on the jump host has password-less SSH access to all app servers through their respective sudo users (for example, tony for app server 1). Perform the steps to set up password-less authentication from thor on the jump host to all app servers.

## Solution

### 1) Generate SSH Key (if not already generated)
Check first:
```bash
ls -la ~/.ssh
```

If you don’t see id_rsa and id_rsa.pub, generate:
```bash
ssh-keygen -t rsa -b 4096
```

Press Enter for all prompts (no passphrase).

This creates:
```bash
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

### 2)Copy Public Key to Each App Server User
```bash
ssh-copy-id tony@172.16.238.10
ssh-copy-id steve@172.16.238.11
ssh-copy-id banner@172.16.238.13
```

### 3)Test Password-less SSH Login

From jump host:
```bash
ssh tony@172.16.238.10
```

It should log in without asking password.

### 4)Make Sure Sudo Doesn't Ask Password
On each app server, check:
```bash
sudo visudo
```

Make sure the user has:
```bash
tony ALL=(ALL) NOPASSWD: ALL
```

Otherwise scripts using sudo will still ask password.

### 5)Verification

From jump host:
```bash
ssh tony@172.16.238.10 "sudo whoami"
```

Expected output:
```bash
root
```

