# Day 8: Install Ansible

## Task

Install ansible version 4.7.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

## Solution

### 1) Install required system packages

```bash
sudo dnf install -y python3 python3-pip
```
### 2) Create a virtual environment

```bash 
python3 -m venv ~/ansible-4.7
source ~/ansible-4.7/bin/activate
```

### 3) Install Ansible 4.7.0 globally using pip3

```bash
sudo pip3 install ansible==4.7.0
```

### 4) Verify installation
```bash
ansible --version
```

