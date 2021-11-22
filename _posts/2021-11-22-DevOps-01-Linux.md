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








