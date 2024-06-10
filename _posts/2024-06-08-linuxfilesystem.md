---
layout: post
title: 08-06-2024
---                                
# Linux File System

Directorie	Description
1.      /bin	binary or executable programs.
2.      /etc	system configuration files.
3.      /home	home directory. It is the default current directory.
4.      /opt	optional or third-party software.
5.      /tmp	temporary space, typically cleared on reboot.
6.      /usr	 User related programs.
7.      /var 	log files.
8.      /boot	It contains all the boot-related information files and folders such as conf, grub, etc.
9.      /dev	It is the location of the device files such as dev/sda1, dev/sda2, etc.
10.     /lib	It contains kernel modules and a shared library.
11.     /lost+found	It is used to find recovered bits of corrupted files.
12.     /media	It contains subdirectories where removal media devices are inserted.
13.     /mnt	It contains temporary mount directories for mounting the file system.
14.     /proc	It is a virtual and pseudo-file system to contains info about the running processes with a specific process ID or PID.
15.     /run	It stores volatile runtime data.
16.     /sbin	binary executable programs for an administrator.
17.     /srv 	It contains server-specific and server-related files.
18.     /sys	It is a virtual file system for modern Linux distributions to store and allows modification of the devices connected to the system.

# Types of file system
- Linux supports a number of native filesystem types, expressly created by Linux developers, such as:
   - ext3
   - ext4
   - squashfs
   - btrfs

   - **XFS**- XFS is a 64-bit journaling file system and was ported to Linux in 2001. It now acts as the default file system for many Linux distributions. It provides features like snapshots, online defragmentation, sparse files, variable block sizes, and excellent capacity.
    - XFS supports metadata journaling, which facilitates quicker crash recovery. 

   - **ext4**- It has backward compatibility with ext3 and ext2 and it provides several other features, some of which are persistent pre-allocation, unlimited number of subdirectories, metadata checksumming and large file size. ext4 is the default file system for many Linux distributions 

   - **btrfs**- It was developed in 2007. It provides many features such as snapshotting, drive pooling, data scrubbing, self-healing and online defragmentation. It is the default file system for Fedora Workstation.
   {% highlight ruby %}
    man fs //to see about filesystem available
    cat /pro/filesystem //to see the available filesystem
   {% endhighlight %}


 # Create and manage partition in linux
  - Disk Partitioning is the process of dividing a disk into one or more logical areas, often known as partitions, on which the user can work separately. It is one step of disk formatting. If a partition is created, the disk will store the information about the location and size of partitions in the partition table. With the partition table, each partition can appear to the operating system as a logical disk, and users can read and write data on those disks

  - To Partition Disks in Linux
       - Attach the disk to the proper port.
       - Create partitions in the disk.
       - Create a file system on the partition.
       - Mounting the file systems 


{% highlight ruby %}
    **commands to do disk partition**
    **Step-1**
    lsblk    //list block devices
    fdisk -l  <disk path> //to see the disk
    //here with the help we can create physical and extended partitions
    **step-2**
    Format with 
   {% endhighlight %}
