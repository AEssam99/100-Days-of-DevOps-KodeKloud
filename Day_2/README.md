# Day 2: Temporary User Setup with Expiry

## Task

Create a temporary Linux user with an expiration date

## Solution
### 1) Create user with expiration date
```bash
sudo useradd -e 2026-03-01 bob
```

### 2) Set password for the user
```bash
sudo passwd bob
```

### 3) Verify expiration date
```bash
sudo chage -l bob
```
