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
    **step-2**-Format with the file system
    mkfs.ext4 <disk path>
    blkid <disk path>   //to see the uuid
    **step-3**-Mount the part
    mkdir <directory path>
    mount <disk path> <mount path>  //but it is not permanent mounting
    **To permanent mounting**
    vim /etc/fstab
    mounted partition[ space ] directory [ space ] file system type [ space ]  defaults  0   0 
    mount -a //to mount partition permanently
    unmount <disk path> <directory path>
    df -h //to see the disk space
   {% endhighlight %}


  **swap memory** - Swap space in Linux is used when the amount of physical memory (RAM) is full. If the system needs more memory resources and the RAM is full, inactive pages in memory are moved to the swap space. While swap space can help machines with a small amount of RAM
     {% highlight ruby %}
    **commands to create space memory**
      lsblk  //to see the swap memory
      mkswap  <disk path>
      swapon  <disk path>
      swapon -a   //to allocate all swap created
     {% endhighlight %}


# NFS(Network file system) 
 - NFS allows a system to share directories and files with others over a network. By using NFS, users and     programs can access files on remote systems almost as if they were local files.
 - NFS can be configured as a centralized storage solution.
 - No need of running the same OS on both machines.
 - Can be secured with Firewalls.
 - The NFS share folder can be mounted as a local file system.

-  NFS mount needed at least two machines. The machine hosting the shared folders is called as server and which connects is called as clients.

**Configuring NFS Server**
{% highlight ruby %}
   suppose IP address Details of Server & Client
   Server: 192.168.87.156
    Client: 192.168.87.158
     {% endhighlight %}
****Configuring Server-side**
      {% highlight ruby %}
         #yum install nfs-utils nfs-utils-lib
         # service rpcbind start
         # service nfs start
          {% endhighlight %}


- We need to decide a directory which we want to share with the client. The directory should be added to /etc/exports      
  {% highlight ruby %}
       # vi /etc/exports
      /share 192.168.87.158(rw,sync,no_root_squash)  //save the file
       - /share – is the share folder which server wants to share
       - 192.168.87.158 – is the IP address of the client to whom want to share
       -  rw – This will all the clients to read and write the files to the share directory.
       -  sync – which will confirm the shared directory once the changes are committed.
       -  no_root_squash – This will all the root user to connect to the designated directory.
        #exportfs -arv  //to notify Linux about the directories you are allowing to be remotely mounted using NFS.
 {% endhighlight %}

 **configuring client-side**

{% highlight ruby %}
 #yum install nfs-utils nfs-utils-lib 
 #showmount -e 192.168.87.156
 #mkdir -p /mnt/share  //create the directory to mount point the shared folder
 #mount 192.168.87.156:/share /mnt/share/    //mounting the shared directory
 #df -h
 {% endhighlight %}

 **Create a file and folders in the server share directory**

- touch test1
-  mkdir test
   - // these files can also be seen on client side
   


   
 



