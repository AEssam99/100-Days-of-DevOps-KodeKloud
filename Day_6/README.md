# Day 6: Create a Cron Job

## Task

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. 
Therefore, perform the steps below:

a. Install cronie package on all Nautilus app servers and start crond service.

b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.

## Solution

### 1) Install the Cronie package in the app-server
```bash 
sudo yum install cronie -y
```

### 2) Create Cron job
```bash
crontab -e
# will open a temporary file/text editor.

# In this file, you can add your cron job.
*/5 * * * * echo hello > /tmp/cron_text

# On saving the file, you get 'crontab: installing new crontab'
# You can try crontab -l to list your cron job.
```



