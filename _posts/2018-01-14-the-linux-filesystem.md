---
layout: post
title: "The Linux Filesystem"
description: Linux Filesystem
tags: linux filesystem
date: 2017-01-14
---

Like all UNIX-based operating systems, Linux consists of a single big filesystem tree. It's designed as an inverted tree with the root directory **/** at the top of the tree.  

Different filesystems, locally present in different partitions, or sometimes on the network, can be **mounted** at various points within this large logical filesystem.  

Regardless of how things are actually joined together, it all just looks like one filesystem, with applications not caring at all about what physical device files actually reside on.  

The following is a list of the main directories normally found under **/**:

### /
This is the primary directory of the entire file system hierarchy.
The partition and filesystem containing the root directory itself is often in a special dedicated partition. The root partition must contain all essential files required to boot the system, and mount all the other file systems.

### /bin
Contains essential executable programs and scripts that must be available in single user mode. These programs are required when no other filesystems have yet been mounted. This directory may not include any subdirectories.

Required programs which must exists in this directory are:  
cat, chgrp, chmod, chown, cp, date, dd, df, dmesg, echo, false, hostname, kill, ln, login, ls, mkdir, mknod, more, mount, mv, ps, pwd, rm, rmdir, sed, sh, stty, su, sync, true, umount and uname. 

Command binaries that are deemed non-essential enough to merit a place in **/bin** go in **/usr/bin**

### /boot
Files needed to boot the system, such as the kernel(**vmlinuz**), **initrd**, **initramfs** and boot configuration files and bootloader programs.

### /dev
Device nodes, which are special files used to interact with hardware and software devices.

### /etc
System wide configuration files and some stratup scripts; there should be no executable binary programs.
File and directories which may be found here include:  

csh.login, exports, fstab, ftpusers, gateways, gettydefs, group, host.conf, hosts.allow, hosts.deny, hosts,equiv, hosts.lpd, inetd.conf, inittab, issue, ld.so.conf, motd, mtab, mtools.conf, networks, passwd, printcap, profile, protocols, resolv.conf, rpc, securetty, services, shells, syslog.conf.  

Other important subdirectories includ **/etc/skel**, and **/etc/init**.


### /home
User **home** directories, including personal settings, file, etc.  

### /lib  
Libraries required by executable binaries in /bin and /sbin.

### /media
Mount points for removable media such as CDs, DVDs, USB Sticks, etc.

### /mnt
Temporarily mounted filesystems

### /opt
Optional application software packages

### /proc
Virtual pseudo-filesystem giving information about the system and processes running on it. Can be used to alter system parameters.

### /sys
Virtual pseudo-file system giving information about the system and processes running on it.

### /root
Home directory of the **root** user.

### /sbin
Essential system binaries.

### /srv
Site-specific data served up by the system.

### /tmp
Temporary files

### /usr
Multi-user applications, utilities and data;

### /var
Variable data the changes during system operation

### Others
The [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html), previously created by Free Standards and now passed onto the Linux Foundation, standardizes the filesystem structure as the above across all distributions.   
We may however find additional distribution-specific dirs found under the root directory.  
These may include things like  **/misc** or **tftpboot**,


