---
layout: post
title: "The Linux Filesystem"
description: Linux Filesystem
tags: linux filesystem notes
date: 2018-01-16
---

This post is really not a post, it's mainly a dump of notes I have been putting together while following one of my online courses related to Linux & OpenSource.  
It has memes so it's a little less boring, but I do understand if one spaces out even then.  
With that out of the way, let's begin!  

![Old fart who likes writing fantasy books about winter and people dying](https://i.imgur.com/L6uT1Mn.jpg)

Like all UNIX-based operating systems, Linux consists of a single big filesystem tree. It's designed as an inverted tree with the root directory **/** at the top of the tree.  
Different filesystems, locally present in different partitions, or sometimes on the network, can be **mounted** at various points within this large logical filesystem.  

Regardless of how things are actually joined together, it all just looks like one filesystem, with applications not caring at all about what physical device files actually reside on.  

The following is a list of the main directories normally found under **/**:

![du](https://i.imgur.com/u8vsKGC.png)

### /
This is the primary directory of the entire file system hierarchy.
The partition and filesystem containing the root directory itself is often in a special dedicated partition. 
The root partition must contain all essential files required to boot the system, and mount all the other file systems.

### /bin
Contains essential executable programs and scripts that must be available in single user mode. 
These programs are required when no other filesystems have yet been mounted. This directory may not include any subdirectories.

Required programs which must exists in this directory are:  
**cat, chgrp, chmod, chown, cp, date, dd, df, dmesg, echo, false, hostname, kill, ln, login, ls, mkdir, mknod, more, mount, mv, ps, pwd, rm, rmdir, sed, sh, stty, su, sync, true, umount and uname.**   

Command binaries that are deemed non-essential enough to merit a place in **/bin** go in **/usr/bin**

### /boot
Files needed to boot the system, such as the kernel(**vmlinuz**), **initrd**, **initramfs** and boot configuration files and bootloader programs.

### /dev
Device nodes, which are special files used to interact with hardware and software devices.

### /etc
System wide configuration files and some stratup scripts; there should be no executable binary programs.
File and directories which may be found here include:  

**csh.login, exports, fstab, ftpusers, gateways, gettydefs, group, host.conf, hosts.allow, hosts.deny, hosts,equiv, hosts.lpd, inetd.conf, inittab, issue, ld.so.conf, motd, mtab, mtools.conf, networks, passwd, printcap, profile, protocols, resolv.conf, rpc, securetty, services, shells, syslog.conf.**  

Other important subdirectories includ **/etc/skel**, and **/etc/init**.

### /home
User **home** directories, including personal settings, files, etc.  
All personal configurations, data, executable programs would be placed under this directory, for example under **/home/sk1u**.

Users can always substitute the environmental variable $HOME for their root directory, or the shorthand **~**:

`$ ls -l $HOME/public_html`

is the same as:

`$ ls -l ~/public_html`

### /lib
Libraries required by executable binaries in /bin and /sbin.
These libraries are particularly important for booting the system and executing commands within the root filesystem.

Kernel modules (like device or filesystem drivers) are located under **/lib/modules/\<kernel-version-number\>**

**PAM** (Pluggable Authentication Modules) files are stored in **/lib/security**.  
A **/lib64** directory may be found in cases of Red-Hat systems that support 64-bit binaries.

### /media
Mount points for removable media such as CDs, DVDs, USB Sticks and floppy disks.  
Modern Linux systems mount such media dynamically upon insertion, with **udev** creating directories under **/media** and mounting the removable filesystems there. Upon unmounting and removal, directories used as mountpoints under **/media** disappear.

### /mnt
Temporarily mounted filesystems.
This directory is available so that administrators can temporarily mount a filesystem when needed. A common use is for network filesystems such as **NFS, Samba, CIFS, AFS**

### /opt
Optional application software packages.
The directory is designed for software packages that wish to keep all or most of their files in one isolated place. This makes installing and uninstalling software relatively easy, everything being in one convenient isolated location.

Using system packaging sisyems such as RPM or APT, makes it also easy to install and uninstall with a clear sense of file manifests and locations.

### /proc
Virtual pseudo-filesystem giving information about the system and processes running on it. Can be used to alter system parameters. This information resides only in memory, not on disk. This directory is empty on a non-running system, and constantly changing when the system is actually running.
Important pseudo-files include **/proc/interrupts, /proc/meminfo, /proc/mounts, /proc/partitions**, and provide an up-to-the-moment glimpse of the system's hardware.

### /sys
Virtual pseudo-file system giving information about the system and processes running on it. This directory resembles **/proc** a lot, but is younger and has adhered to strict standards about what kind of entries it can contain. Unlike **/proc**, all pseudo-files in **/sys** contain only one line.
The directory itself is the mount point of the sysfs pseudo-filesystem where all information resides only in memory.

### /root
This is the Home directory of the **root** user.

### /sbin
Essential system binaries, used for booting, restoring, recovering and/or repairing the system, in addition to the ones in **/bin**.

Examples of binaries included in this directories are the following:  
**fdisk, fsck, getty, halt, ifconfig, init, mkfs, mkswap, reboot, route, swapon, swapoff, update.**

### /srv
Site-specific data which is served by the system.
The main purpose of specifying this is so users can find location of the data files for a particular service. There is no general consensus at the moment as to how this directory should be structured. One method be to structure data by protocol, example **ftp, rsync, www and cvs**.

### /tmp
Temporary files which can be accessed by any user or application.  
Data in this directory is not meant to be depended on to stay around for a long time.

### /usr
Multi-user applications, utilities and data;
It can be thought of as a secondary hierarchy. It's used for files which are not needed for system booting. Software packages should not create subdirs directly under **/usr**. symbolic links may exist to other locations for compatibility purposes. The directory itself is typically read-only; It contains binaries which are not needed in single user mode.

### /var
Variable data the changes during system operation, such as log files, printing queues, mail queues, administrative data files, cache contents.
It is often considered good practice to mount /var as a separate filesystem, as it cannot be mounted read-only.
**/var/log** and **/var/spool** are notable directories which are the most used. 

### /run
Another pseudo-filesystem existing only in memory.  
Its purpose is to store transient files, such as those containing runtime information, and which do not need to be preserved when rebooting.
In generalm **/run** is implemented as an empty mount point, with a tmpfs ram disk mounted there at runtime.
This directory has been in use for several years by major Linux distributions, but still not formally accepted as a standard.

### Others
The [Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html), previously created by Free Standards and now passed onto the Linux Foundation, standardizes the filesystem structure as the above across all distributions.   
We may however find additional distribution-specific dirs found under the root directory.  
These may include things like  **/misc** or **/tftpboot**,

Finally, I leave you with scumbag linux guy to wrap up this mess:

![Linux Douchebag](https://i.imgur.com/vsVhLCn.jpg)


