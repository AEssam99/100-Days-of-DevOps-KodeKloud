# Day 2: Temporary User Setup with Expiry

## Task

Task: Create a temporary user account with an expiry date for a developer requiring limited-duration access to the system.

Specific Requirements:

- Create user bob on App Server 2
- Set expiry date to 2026-03-01
- Follow standard protocol: username in lowercase
- Ensure proper validation of configuration
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
