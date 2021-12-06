---
title: DevOps learning journey 01 - Linux
layout: post
subtitle: null
date: '2021-11-22 22:40:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- English
- wiki
---
# 1 Linux introduction

## 1.1 History of Unix & Linux
### 1.1.1 UNIX development history
- UNIX is like the father, Linux is like the son. They share a lot of similarities (dir structure, commands and management)
- Multics(1960s) → UNIX(1969) → UNIX in C(1973) → BSD(1977) → *GNU project* → Minix(1987) → Linux(1991)
(OS history graph)
- Linux: Unix-like systems

### 1.1.2 Linux history and different versions
- Linux history
  - 1991, Finland, University of Helsinki, Linus Torvalds
  - Minix (Andrew Tanenbaum) → Linux (Linus Torvalds)
  - Linux: Free, open source OS, released under GNU General Public License v2
    - One of the most prominent example of free & open-source software collaboration
- Linux kernel version
  - Kernel: The essential center of a computer OS. The core that provides basic services for all other parts of the OS. Main layer between the OS and hardware. Helps with process and memory management, file systems, device control etc.
  - www.kernel.org
  - Latest: 5.13.11
- Linux distributions:
  - RPM-based
    - Redhat
    - CentOS
    - Fedora
    - SuSE
    - ...
  - DEB-based
    - Debian
    - Ubuntu
    - KNOPPIX
    - ...
  - Difference: How to install software

## 1.2 Open source software
- Open source software:
  - free(no $)
  - free to research - source code
  - free to improve & distribute - e.g. commercial & non conmmercial distribution
  - safety - (lots of 👁 watching!)
- "free software" - free for freedom
- LAMP: Linux, Apache, MySQL, PHP

## 1.3 Linux application
(Linux structure graph)
1. Enterprise server
  - www.netcraft.com
  - A lot of websites server OS is Linux
  (Market share for top servers graph)
2. Embedded Computer
  - Mobile, tablet PC, Android - Linux
  - Smart Home appliances
  - karaoke machine
3. Linux in Film production
  - Film production tools available on GNU/Linux
  - Filmmakers: Dreamworks, Pixar, WetaDigital etc.
  - Movies: Titanic, Harry Potter, Finding Nemo, Toy story, Avatar etc.

---
Linux Principles

- Everything is a file
    - Network settings...
    - Commands
    - Including hardware
        - Hard Disk Drive：`/dev/sd[a-p]`
        - CD/DVD ROM：`/dev/sr0`
- Small, single-purpose programs
- Ability to chain programs together to perform complex tasks
- Avoid captive user interfaces
- Configuration data stored in a text file

---

# 2 Linux installation
- Windows: Drive Partition → Format → Assign drive letter
- Linux: Drive Partition → Format → Create the device file → Mount
- Device file name:
    - IDE hard drive: `/dev/hd[a-d]`
    - SCSI/SATA/USB: `/dev/sd[a-p]`
    - floppy disk: `/dev/fd[0-1]`
    - mouse: `/dev/mouse`
    - ...

---
# 3 Linux dir structure
(dir structure graph)
- / → The top-level directory is the root filesystem
    - Contains all of the files required to boot the operating system before other filesystems are mounted
    - as well as the files required to boot the other filesystems.
    - After boot, all of the other filesystems are mounted at standard mount points as subdirectories of the root.
- /bin → binaries (command binaries)
    - commands in bin can be used in single-user mode
- /sbin
    - super essential commands, administrative tools only available to the root
- /usr → also has /bin and /sbin
    - a lot of similarities with /bin and /sbin in root directory.
    - But contain more commands
    - If we want to know where does the command binary live — use `which`
    - /local: also store command binaries but only user created binaries.
    - /games → lolcat e.g. `cal | lolcat`
- /boot → boot files
- /var → log files, web application related files
- /tmp → temporary files
- /lib → shared library that are required for system boot.
- /home → contains users
- /root → root user
- /dev → device, e.g. sda1, sda2 (drive names)
- /etc → "etcetera" (etsy files) settings, e.g. network settings
    - e.g. `cat /etc/sysconfig/network-scripts/ifcfg-ens33`
- /mnt → mount e.g. drive you mount manually
- /media → both mount drives, e.g. /media automatically mount usb
- /lost + found → Recover files
    - Any corrupted files found will be placed in the lost+found directory.

- FHS in Linux: Filesystem Hierarchy Standard file system structure
- mount → /etc/fstab (file system table)
    - fstab is a configuration table designed to ease the burden of mounting and unmounting file systems to a machine. It is a set of rules used to control how different filesystems are treated each time they are introduced to a system.

---
# 4 Linux Commands
## 4.1 File management commands

### 4.1.1 ls command

`ls -la /etc`

- -a: all
- -l: long
- -h: human (e.g. 105K size)
- `-rw-r--r--. 1 root root 1787 May 11 2019 request-key.conf`
    - 1: reference time
    - 1st root: owner
    - 2nd root: group
    - `1787`: byte, size
    - `May 11 2019`: last modified time
    - `request-key.conf`: file name
    - `-rw-r--r--`:
        - `-`: file type， - binary，d directory，l symbolic link
        - u user
        - g group
        - o others
        - minimum permission
    - `ls -d`: read a directory's info
    - `ls -i`: see inode
        - `cd /etc/sysconfig/network-scripts/`
        - `ls -i ifcfg-ens33`

### 4.1.2 directory management command

- mkdir: make directories
    - `cd tmp`
    - `mkdir movies`
    - `mkdir music/taylor` (wrong!)
    - `mkdir -p music/taylor`
        - -p parents
        
- cd: change directory
- pwd: print working directory
- rmdir: remove empty directories
- cp: copy
    - `touch loveStory`
    - `cp loveStory blankSpace`
    - -r (recursive), can copy directory
    - -p keep properties
- mv: move
    - can cut and move a lot of files to a same directory
    - can rename a file
    - `mv blankSpace badBlood`
- rm: remove
    - rm -rf [file or directory]
    - -r delete directory
    - -f force delete
    - -i ask before delete
    - `rm -i badBlood`
    - `ls`

### 4.1.3 File processing commands

- touch: create a file
    - can create a file in relative path or absolute path
    - an absolute path refers to the same location in a file system relative to the root directory, whereas a relative path points to a specific location in a file system relative to the current directory you are working on.
    - `touch "blank space"`
- cat: concatenate
    - -n show line number
    - `cat -n /etc/services`
    - show all the lines at the same time, not suitable for long files
- tac:
    - show file but upside down
    - `tac /etc/services`
- more:
    - space or f for next page
    - Enter for next line
    - b for previous page
    - q for quit
    - better for long files with pagination
    - `more /etc/services`
- less
    - `less /etc/services`
    - very similar to more
    - can use up arrow or down arrow
    - can find `/service` keyword
    - keep press n for finding more "service" keyword
- head
    - `head -n 20 /etc/services`
    - by default 10 lines
- tail
    - `tail -n 18 /etc/services`
    - `tail -f var/log/messages` (f for follow)

### 4.1.4 link commands

- ln: link
- `ln -s /etc/issue /tmp/issue.soft`
- `ln /etc/issue tmp/issue.hard`
    - hard link is a lot like cp -p
    - hard link properties and inode is the same as original file

## 4.2 Permission commands

### 4.2.1 Permission command chmod

- chmod: change the permission mode of a file
- e.g. `chmod u+x, o-r Nanjing.list` （not working on CentOS 8）
- e.g. `chmod g=rwx Nanjing.list` (working)
- `chmod 640 Nanjing.list`
- rwx means different things for a file and for a directory.
    - for directory:
        - r = ls
        - w = touch, mkdir, rmdir and rm in it
        - x = can cd into it
- chown: change file ownership
- chgrp: change file group ownership
- useradd
- passwd
- umask: the user file — creation mask, set default file permission

## 4.3 File search command
### 4.3.1 File search command find
- Better not use `find`, arrange your files wisely instead because `find` may consume lots of resource
- the search range: better be small & accurate
- name: `find`
- path: `/bin/find`
- permission: all users
- use: `find [range] [conditions]`
- function: search files
- search methods:
  - file name:
    - `find /etc -name init`
    - `find /etc -name *init*`
    - `find /etc -name init???`
    - `-iname` not case sensitive
  - file size:
    - `find / -size +204800` search for files larger than 100MB under root dir
    - `find / -size +100M`
  - owner or group:
    - `find /home -user lz`
    - `find /home -group dev`
  - time:
    - `find /etc -cmin -5` find files or directories that the meta data was modified within 5 min under etc dir
    - `-amin` access time
    - `-cmin` meta data modification time
    - `-mmin` file content modification time
- special flags:
  - `-a` both confitions are true
  - `-o` or, either one

### 4.3.2 Other search command
- locate:
  - path: /usr/bin/locate
  - function: search file by their filename
  - e.g. `locate inittab`
  - if can't find, try `updatedb`
  - `locate -i teacher.txt` not case sensitive

- which:
  - path: /usr/bin/which
  - function: search command directory and alias
  - e.g. `which ls`

- whereis:
  - path: /usr/bin/whereis
  - function: search command directory and mannual path
  - e.g. `whereis ls`

- grep:
  - path: bin/whereis
  - use: `grep -iv [str] [file]`
  - function: search line that contains string in a file
    - `-i` not case sensitive
    - `-v` eliminate string
  - e.g. `grep mysql /root/install.log`
  - `more /etc/inittab`
  - `grep multi /etc/inittab`
  - `grep -v ^# /etc/inittab` will eliminate all the lines that start with #

## 4.4 Help command
- man:
  - use: `man [command or config files]`
  - function: get help info
  - e.g. `man ls`
  - e.g. `man services` equal to more /etc/services
  - reading mode is similar to `more`, f, b, up, down, q (space to change page, Enter to continue reading line by line)

- whatis:
  - `whatis ls`
  - `whatis ifconfig`
  - function: get short command introduction

- apropos:
  - `apropos services`
  - `apropos inittab`
  - function: can read intro of config files

- help:
  - function: can check Shell language grammar & get Shell commands
  - e.g. `help if`

- help option: `--help`
  - use this after commands if you don't want to read intro but want to know more about options 

## 4.5 User management command
- useradd:
  - function: add new users
  - e.g.`useradd lz`
  - permission: root

- passwd:
  - function: set password
  - e.g. `passwd lz`

- who:
  - function: check how many users are logging in

- w:
  - function: check detailed info of log-in users
  
## 4.6 Zip/Unzip command
- gzip:
  - function: zip files
  - file format after `gzip`: .gz
  - `gzip` can only compress file, not directories.
  - after `gzip` the original file is not there any more.

- gunzip:
  - function: unzip .gz files
  - e.g. `gunzip homework.gz`
  - or we can use `gzip -d`

- tar
  - use: `tar option[-zcf] [filename] [directory]`
    - `-c` pack
    - `-v` show info
    - `-f` use filename
    - `-z` pack and zip
  - function: pack directories
  - file format after zip: .tar.gz

  - tar (unpack & unzip)
    - tar command options:
      - `-x` unpack
      - `-v` show info
      - `-f` filename
      - `-z` unzip
    - e.g. `tar -zxvf homework.tar.gz`

- zip
  - use: `zip option[-r] [filename] [file or directory]`
    - `-r` zip directory
    - e.g. `zip -r homework.zip Homework`
  - function: zip file or directory
  - original file will be kept

- unzip
  - use: `unzip [file]`
  - function: unzip .zip file
  - e.g. `unzip test.zip` 

- bzip2
  - use: `bzip2 option[-k] [file]`
    - `-k` create compressed file and keep the original file
  - function: compress file
  - file format after `bzip2`: .bz2
  - e.g. `bzip2 -k Homework`
  - e.g. `tar -cjf Homework.tar.bz2 Homework`
    - tar -j is to create bz2 format
  - the compression ratio is high

## 4.7 Network command

---

Useful Resources:

[Linux for Hackers // EP 1 (FREE Linux course for beginners)](https://www.youtube.com/watch?v=VbEx7B_PTOE&t=15s)

[Linux for Hackers // EP 2 (the Linux File System)](https://www.youtube.com/watch?v=A3G-3hp88mo&t=6s)

中文教程：

[史上最牛的Linux视频教程—兄弟连](https://www.bilibili.com/video/BV1mW411i7Qf?share_source=copy_web)




