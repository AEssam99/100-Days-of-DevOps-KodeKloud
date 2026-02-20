#Day 10: Linux Bash Scripts

##Task
The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 2 in Stratos Datacenter, and they need to create a bash script named blog_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 2).



a. Create a zip archive named xfusioncorp_blog.zip of /var/www/html/blog directory.


b. Save the archive in /backup/ on App Server 2. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.


c. Copy the created archive to Nautilus Backup Server server in /backup/ location.


d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.


e. Do not use sudo inside the script.


##Solution

###1) Install zip on App Server 2 (outside the script)
```bash
sudo yum install -y zip unzip
```

###2) Enable passwordless copy to Nautilus Backup Server (SSH key)
Run these as the App Server 2 user:

####Generate SSH key (press Enter for all prompts):
```bash
ssh-keygen -t rsa -b 4096
```

####Copy the public key to Nautilus Backup Server user:
```bash
ssh-copy-id clint@172.16.238.16
```

####Quick test (should NOT ask for password):
```bash
ssh clint@172.16.238.16 "ls -ld /backup"
```

###3) Create the script on App Server 


####Create the script on App Server 2:
```bash
mkdir -p /scripts
vi /scripts/blog_backup.sh
```

####Paste this script:
```bash
#!/bin/bash
set -euo pipefail

SRC_DIR="/var/www/html/blog"
LOCAL_BACKUP_DIR="/backup"
ARCHIVE_NAME="xfusioncorp_blog.zip"
LOCAL_ARCHIVE_PATH="${LOCAL_BACKUP_DIR}/${ARCHIVE_NAME}"

REMOTE_USER="clint"
REMOTE_HOST="172.16.238.16"
REMOTE_DIR="/backup"

# Ensure source exists
if [[ ! -d "$SRC_DIR" ]]; then
  echo "ERROR: Source directory not found: $SRC_DIR"
  exit 1
fi

# Ensure local backup directory exists
mkdir -p "$LOCAL_BACKUP_DIR"

# Create zip archive (from inside /var/www/html so the zip contains blog/...)
cd /var/www/html
rm -f "$LOCAL_ARCHIVE_PATH"
zip -r "$LOCAL_ARCHIVE_PATH" "blog" >/dev/null

# Copy to Nautilus Backup Server (must be passwordless via SSH keys)
scp -q "$LOCAL_ARCHIVE_PATH" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}/"

echo "Backup completed:"
echo " - Local:  $LOCAL_ARCHIVE_PATH"
echo " - Remote: ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_DIR}/${ARCHIVE_NAME}"2: /scripts/blog_backup.sh
```
###4) Make it executable (and runnable by the server user)
```bash
chmod 755 /scripts/blog_backup.sh
```

###5) Run + verify
```bash
/scripts/blog_backup.sh
ls -l /backup/xfusioncorp_blog.zip
```

Verify on backup server:
```bash
ssh clint@172.16.238.16 "ls -l /backup/xfusioncorp_blog.zip"
```
