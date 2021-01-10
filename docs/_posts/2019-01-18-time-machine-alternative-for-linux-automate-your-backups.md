---
title: "Time Machine alternative for linux: automate your backups"
date: "2019-01-18"
categories: 
  - "scripts"
tags: 
  - "backup"
  - "linux"
---

**Introduction**

MacOS has a good backup system which allows you to recover any files that have beed deleted or to restore previous versions of the files. I works like you can literally come back into a certain moment back in time.

Now that I have set up Raspberry Pi based home NAS, I found a solution that achieves the same results on linux: incrental backups with rsync.

**Instruction: how to automate backups on Linux**

**Step 1. Set up rsync backup script, with instructions and settings on how to run the backup**

```
sudo mkdir /usr/local/scripts      #to make folder to store scripts
sudo touch /usr/local/scripts/backup.sh # to create the script
sudo chmod +x  /usr/local/scripts/backup.sh  # make it executable
sudo nano /usr/local/scripts/backup.sh  #THEN EDIT THE SCRIPT:
```

Copy this code into the script:

	# This script makes backups from DATADIR to BACKUPDIR and puts in 
	# date-named folders corresponding to date of each backup
	# SYNTAX:  $ ./backup.sh </DATADIR/ ($1)> </BACKUPDIR/ ($2)>
	# Example: $ ./backup.sh /media/data/ /media/backup/
	
	# To be able to use CTRL+C to exit script:
	trap "exit" INT 
	
	# Define the directories for backup
	DATA=$1
	LAST_BACKUP_PATH=$2/$(ls $2 | tail -n 1)
	THIS_BACKUP_PATH=$2/backup_in_progress
	
	# Make the backup
	rsync -av --link-dest ${LAST_BACKUP_PATH} ${DATA} ${THIS_BACKUP_PATH}
	
	# Move the backup to date-named folder after it's finished
	mv $2/backup_in_progress $2$(date +%Y-%m-%d)


**Step 2. Set up cron job to run the backup script on a schedule**

	sudo crontab -e


To backup DATA\_DIR to BACKUP\_DIR at 3am every night, add this to your cron file:


	# BACKUP SCRITPS
	# WHEN       SCRIPT DIR                    DATA_DIR     BACKUP_DIR
	0 4 1 * *  /usr/local/scripts/backup.sh  /data/dir/  /backup/dir
	0 3 * * *  /usr/local/scripts/backup.sh  /data2/dir/ /backup2/dir
	# - - - - - CRON SYNTAX explained below:
	# | | | | |
	# | | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
	# | | | ------- Month (1 - 12)
	# | | --------- Day of month (1 - 31)
	# | ----------- Hour (0 - 23)
	# ------------- Minute (0 - 59).  

	
	fsf
	dfdf



**Useful Links**

More on cron read [here][1], or crontab calculator [here][2]

Bash scripts [101][3]

[https://linuxconfig.org/how-to-create-incremental-backups-using-rsync-on-linux][4]

[http://www.mikerubel.org/computers/rsync\_snapshots/][5]

[https://stackoverflow.com/questions/13778889/rsync-difference-between-size-only-and-ignore-times][6]

[https://unix.stackexchange.com/questions/102211/rsync-ignore-owner-group-time-and-perms][7]

[1]:	https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/
[2]:	https://crontab.guru/
[3]:	https://medium.com/tech-tajawal/writing-shell-scripts-the-beginners-guide-4778e2c4f609
[4]:	https://linuxconfig.org/how-to-create-incremental-backups-using-rsync-on-linux
[5]:	http://www.mikerubel.org/computers/rsync_snapshots/
[6]:	https://stackoverflow.com/questions/13778889/rsync-difference-between-size-only-and-ignore-times
[7]:	https://unix.stackexchange.com/questions/102211/rsync-ignore-owner-group-time-and-perms