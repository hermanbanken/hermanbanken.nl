---
title: Backup Network Homedirectories to Time Machine
author: hbanken
type: post
date: 2011-01-23T17:22:16+00:00
excerpt: "Having a home server can be very useful when having a lot of computers at home or having a lot of machines with different capabilities. User can then choose which computer they want to use and will always have there home directory, group folders and shares right at the dock. Very convenient. But then you start to think about redundancy and data safety. What happens if your single home server crashes. All files and a lot of work will be lost. So there is a need for backups and since we're using Mac OS X Server I started to look at Time Machine for backing up home directories."
url: /2011/01/23/backup-network-homedirectories-to-time-machine/
categories:
  - Apple
  - OSX
tags:
  - backup
  - data security
  - homeserver
  - redundant backups
  - Time Machine

---
[<img class="size-full wp-image-229 alignright" style="border: none; padding: 0 0 0 5px; background: none;" title="TimeMachine logo" src="/images/2011/01/TimeMachine-Logo.png" alt="TimeMachine logo" width="145" height="145" srcset="/images/2011/01/TimeMachine-Logo.png 256w, /images/2011/01/TimeMachine-Logo-150x150.png 150w" sizes="(max-width: 145px) 100vw, 145px" />][1]Having a home server can be very useful when having a lot of computers at home or having a lot of machines with different capabilities. User can then choose which computer they want to use and will always have their home directory, group folders and shares right at the dock. Very convenient. But then you start to think about redundancy and data safety. What happens if your single home server crashes. All files and a lot of work will be lost. So there is a need for backups and since we&#8217;re using Mac OS X Server I started to look at Time Machine for backing up home directories.

<!--more-->The Time Machine backend is nothing more than a 

[rbackup][2] over rsync storing files not in directories but in Sparse Images. The client however has a lot to offer. Apple has added the usual eye candy and restor

ing or viewing old versions of files, directories or even program states is easy and almost fun to do. But Time Machine is written to backup from one machine to a backup disk and when configuring Time Machine on a client all data on the client will be backed up, but not the home directories that remain on the server (mobile homes will be backed up).And Time Machine on OS X Server is nothing more than a service that propagates the names of the backup shares, enabling zeroconf stuff/Bonjour for the clients.

<img class="alignright size-full wp-image-230" style="border: none; padding: 0 5px 0 0; background: none;" title="TimeMachine client view" src="/images/2011/01/timemachine_hero20090608.png" alt="TimeMachine client view" width="440" height="324" srcset="/images/2011/01/timemachine_hero20090608.png 440w, /images/2011/01/timemachine_hero20090608-300x221.png 300w" sizes="(max-width: 440px) 100vw, 440px" /> 

What we want is that the user logs in to a client, viewing the current state of the home directory. And when he realizes he needs a document he deleted yesterday he can click Time Machine and restore the file. But the situation now is that the user will only see files that where on the client yesterday and not the files in his home directory.

At the moment I don&#8217;t have a definite solution to this problem but when I do I will update this post. When anyone knows how to solve this, please contact me or better, leave a comment below.

Maybe the solution could be found in setting up a symbolic link on each client to the home directories and a sparse image for each user inside it&#8217;s home directory (excluding .sparseimage&#8217;s from the backup plan) and configuring Time Machine to backup the symbolic link&#8217;s source to the sparse image in the home directory or a special share on the server for these sparse images. But I have not confirmed this setup, and do not now if it will work. I will test this in the future, when I have some spare time.

More about this later.

 [1]: /images/2011/01/TimeMachine-Logo.png
 [2]: http://www.leftmind.net/projects/rbackup/