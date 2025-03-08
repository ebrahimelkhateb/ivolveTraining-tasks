# MySQL Backup Automation 

## Overview

This project sets up an automated MySQL backup system on a Rocky Linux server. A Bash script performs the backup, and a cron job schedules the process to run every Sunday at 5:00 PM.

## Features

- Backs up all MySQL databases.
- Compresses the backup file to save storage.
- Stores backups in a dedicated directory.
- Scheduled automation using cron.

---

## Installation Steps

### 1. Update the System

Run the following command to update all packages:

```bash
sudo dnf update -y
```

### 2. Install MySQL Client

Ensure the MySQL client is installed:

```bash
sudo dnf install mysql-server -y
```

### 3. Create Backup Directory

Set up a directory to store the backup files:

```bash
sudo mkdir -p /backup/mysql
sudo chmod 700 /backup/mysql
```

### 4. Create the Backup Script

1. Create the script file:

   ```bash
   sudo nano /usr/local/bin/mysql_backup.sh
   ```

2. Add the following script:

   ```bash
   #!/bin/bash

   # Variables
   BACKUP_DIR="/backup/mysql"
   DATE=$(date +%Y%m%d_%H%M)
   MYSQL_USER="root"
   MYSQL_PASSWORD="your_password"

   # Perform the backup
   mysqldump -u$MYSQL_USER -p$MYSQL_PASSWORD --all-databases > $BACKUP_DIR/mysql_backup_$DATE.sql

   # Compress the backup
   gzip $BACKUP_DIR/mysql_backup_$DATE.sql
   ```

3. Save and exit the file.

4. Make the script executable:

   ```bash
   sudo chmod +x /usr/local/bin/mysql_backup.sh
   ```



### 5. Set Up Cron Job

1. Edit the root user's cron table:

   ```bash
   sudo crontab -e
   ```

2. Add the following line to schedule the job every Sunday at 5:00 PM:

   ```bash
   0 17 * * 0 /usr/local/bin/mysql_backup.sh
   ```

3. Save and exit.

### 6. Test the Script

Run the script manually to verify:

```bash
sudo /usr/local/bin/mysql_backup.sh
```

Check the `/backup/mysql` directory for the backup file.

---

## Directory Structure

```
/backup/mysql
  ├── mysql_backup_YYYYMMDD_HHMM.sql.gz
```

---

## Notes

- Replace `your_password` with the MySQL root password in the `.my.cnf` file.
- Ensure sufficient disk space for storing backups.
- Periodically check the cron logs for errors using:
  ```bash
  sudo journalctl -u cron
  ```

---


