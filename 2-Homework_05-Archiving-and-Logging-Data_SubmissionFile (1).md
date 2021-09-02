#### Critical Analysis Question

#- Why wouldn't you use the options `-x` and `-c` at the same with `tar`?
	-x is to extract the tar file while -c is to create it.



#1. Cron job for backing up the `/var/log/auth.log` file:

0 6 * * 3 tar -czvvf auth_backup.tgz /var/log/auth.log

##1. Brace expansion command to create the four subdirectories:

#!bin/bash
free -th > ~/backups/freeman/free_mem_usage.txt
df -h > ~/backups/diskuse/disk_usage.txt
lsof > ~/backups/openlist/open_list.txt
top > ~/backups/freedisk/free_disk.txt
---

### Step 4. Manage Log File Sizes
/var/log/auth.log {
	rotate 180
	weekly
	notifempty
	compress
	delaycompress
	endscript
}
