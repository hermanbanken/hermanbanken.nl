---
title: '[OS X] Linux Open Directory and Mavericks clients: network accounts need GeneratedUID field'
author: hbanken
type: post
date: 2014-03-31T14:55:52+00:00
url: /2014/03/31/os-x-linux-open-directory-and-mavericks-clients-network-accounts-need-generateduid-field/
categories:
  - Apple
  - OpenLDAP
  - OSX

---
It has been quite some time since the last post on Open Directory on this blog, but I wanted to share something i found out about OS X Mavericks as a OD client. Let&#8217;s face it: running a non-Apple server for Apple clients is not your ideal, one-click-solution. It is not officially documented and the blogs are sparse, and more importantly: dated. Most how-to&#8217;s date back to 2009 or even older. My own write up dates back from 2009, rewritten in 2011.

A lot has changed since then. The server we tried to mimic was Tiger or Leopard and we tried to hook up clients of the same version. Apple continuously updates OS X and our consumers (or family members) want the latest and greatest features. These new versions like Lion and Mavericks brought some breaking changes. Think about technologies like using AFP in favor of NFS and the now new SMB2 that is not yet fully compatible with Samba, but also the discontinued Workgroup Manager (at least it is not shipped anymore) that I used to model changes from my old Snow Leopard server to my Linux OD&#8217;s autoconfig plist.

[<img class="alignright size-full wp-image-474" alt="Mac OS X needs to repair your Library to run applications. Type your password to allow this." src="/images/2013/12/repair-library.png" width="355" height="192" srcset="/images/2013/12/repair-library.png 355w, /images/2013/12/repair-library-300x162.png 300w" sizes="(max-width: 355px) 100vw, 355px" />][1]Some of my family members are experiencing a lot of trouble with their network home directories continuously disconnecting. I tried to overcome this issue by updating to the newest version of Netatalk (3.1) but this did not solve the problem. I also tried to disable Spotlight as is described \[here\](link needed). No luck. Looking through the log files I found some notice of &#8220;too many open files&#8221;, but searching this on the web I could not link it to network accounts with afp homes on linux servers.

Previously I did not wan&#8217;t our machines to keep local copies of the users home folders as I was worried about the home directories getting out of sync as this behavior can be very hard to explain to my users. When trying mobile accounts my own home folder got out of sync and it was a pain to merge everything back together.

This situation (with a continuously disconnecting home folder) however, was unworkable for the user and I decided to give it one more try. I went to System Preferences > Users and Groups and clicked the button to create a Mobile Account for my user. OS X logged out, and asked for the users password. After that: nothing. Inspecting the logs I found a warning about a missing GeneratedUID field in the user&#8217;s metadata. Looking at the LDAP schema I indeed found an apple-generateduid field that all of my users lacked. I added a binding for this field in my OD config and added the field to all my users. I then tried creating a Mobile Account and suddenly OS X started syncing the home folder! Success!

Not all files got synced immediately &#8211; the home folder of the user is 10G+ &#8211; and after login the home folder was incomplete. Just make sure you sync your users home directory completely or explain them their home still needs to be synced, otherwise they will be scared when they see their empty home directory.

 [1]: /images/2013/12/repair-library.png