---
title: linux
description: 
published: true
date: 2024-01-02T16:09:52.922Z
tags: 
editor: markdown
dateCreated: 2024-01-02T15:50:54.442Z
---

# Linux
# General
## Bash
Restore Overwritten Bash
chsh username -s /bin/sh reboot your system, now you can login successfully and you’ll have a dash shell, reinstall your bash:

sudo apt-get install --reinstall bash then change your default shell to bash:

sudo chsh username -s /bin/bash

When you still have a running terminal: As a bonus, if you ever removed a program which has a running instance you can easily recover it from “procfs”, in case of bash if you had a terminal running bash you could fix the bash by running:

sudo cp /proc/$$/exe /bin/bash

### Source:
https://askubuntu.com/questions/935528/how-do-i-recover-a-deleted-or-overwritten-bash

## Permission:
### Read Permission
For a folder with the files in it
ls -al /FolderPath

### Group Permission
#### Read Group Permission
Specify the user from whom you want to read the group membership
groups User

#### Grant Permission to Group
sudo useradd -G Group User

#### Delete Permission on Group
sudo gpasswd --delete User Group

## Cronjob
There are many examples, but as a reminder for OSTicket a cron job is set up as follows
Example should run the php script cron.php every 5 minutes with the user nobody.
```
user@server:~$ nano /etc/cron.d/osticket 

*/5 * * * * nobody php
/var/www/pathosticket/api/cron.php
```

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
```

<span class=“mw-headline” id=“bkmrk-quelle%3A-3”>Source:</span>
<a class="external free" href="https://en.wikipedia.org/wiki/Cron" rel="nofollow">https://en.wikipedia.org/wiki/Cron</a>

## Disk Size
The command df is used
e.g. df or df /dev/sda1
To display the disk in gigabytes use the following command
df -BG
Also a good switch is -h, here the overview and size is automatically set.
df -h

## Folder Size
The command du is used
For example, to find out the total size of a folder
du -sh /var/lib/Folder

### Source:
http://www.kavoir.com/2009/09/linux-check-how-much-disk-storage-each-directory-takes-up-disk-usage-command-du.html

## Expand Disk
Unfortunately, I have always created my disk with LVM and when it came to a larger disk I have always set up the complete machine anew.
If you are not the Linux professional and are still used to the simple expansion of a virtual disk in Windows, you would like to blow up the internet with all the different instructions and commands.
My way, which at least worked for me with Ubuntu 18.04 and was relatively simple, is as follows.
A pre-check of the hard disk can be performed with the following command sudo lsblk
If it was determined with this command that the disk lying above the LVM has already been expanded, you can skip all points up to point 7.
