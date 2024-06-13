---
layout: post
title: 10-06-2024
---   

#     RSYSC & SCP


**scp** - scp commands is used to copy files between servers in a secure way.
- It is used to transfer data between local host and remote host or between two remote hosts

- Copy all files in a remote directory to a local directory:

 **scp -r username@server_ip:/path_to_remote_directory/* local-machine/path_to directory/**

- Copying one file from one remote server to another remote server
 **scp username@server1_ip:/path_remote_file username@server2_ip:/path_destination_directory/**

**RSYNC**
 - Rsync or remote synchronization,it copies only the difference between the source files present in the local-host and the existing files in the destination or the remote host.

 rsync -v local-file user@remote-host ip:remote-file



 # file permissions in linux
  {% highlight ruby%}
   r-read
   w-write
   x-execute

   **levels of permission**
   u-current user
   g- group user belong to
   o- others
   a-all
   -rwxrwxrwx   normal file
   drwxrx---   directory 
   lrwxrw---  link file
  {% endhighlight %}

  **Tochange file permissions in linux**
  - chmod u+r <filename>
  - chmod u-r <filename>
  - chmod ugo+r <filename>
  - chmod a+w <filename>

  **chmod in numeric mode**
  -  for example - chmod 756 <filename>
  - 0-no permission
  - 1- x(execute)
  - 2 -w(write)
  - 4-r(read)
  - 3- x+w
  - 5 -r+x
  - 6 -r+w
  - 7-r+w+x

  - the default permission for directory-755
  - the default permission for file - 644
   
