---
title: Control your home server
author: hbanken
type: post
date: 2011-12-19T16:31:42+00:00
excerpt: |
  When you have this (or your own working LDAP server) up and running your users can login on Mac OS X and Ubuntu and can use their home directories and stuff. But they can't change their credentials and user info, till now..
  
  LDAP Control is a control centre for your Home Server where your users can manage their data (feature #2) but can also start and plan backups (feature #1), browse your families Address Book (feature #3), manage your favorite series and movies through the media centre (feature 4), and much more (when you guys help me).
url: /2011/12/19/control-your-home-server/
categories:
  - No category

---
Last year I published my Ultimate Single Sign On tool for setting up a perfect Home Server. The work on this project is not yet done and I can use all the help you guys can offer me. Setup is easy: just use VirtualBox, Ubuntu Server and install after a git clone of [git://github.com/hermanbanken/Ultimate-Single-Sign-On-Enviroment-Installer.git][1]. [<img class="alignright size-full wp-image-367" title="Login LDAP control" src="/images/2011/12/Screen-Shot-2011-12-19-at-17.22.47.png" alt="" width="252" height="221" />][2]

When you have this (or your own working LDAP server) up and running your users can login on Mac OS X and Ubuntu and can use their home directories and stuff. But they can&#8217;t change their credentials and user info, till now..

[LDAP Control][3] is a control centre for your Home Server where your users can manage their data ([feature #2][4]) but can also start and plan backups ([feature #1][5]), browse your families Address Book ([feature #3][6]), manage your favorite series and movies through the media centre [(feature 4)][7], and much more (when you guys help me).  
<!--more-->

## List scheduled backups

[<img class="aligncenter size-full wp-image-369" title="Plan backups and manage settings" src="/wp-content/uploads/2011/12/Screen-Shot-2011-12-19-at-17.23.03.png" alt="" width="1031" height="325" srcset="/images/2011/12/Screen-Shot-2011-12-19-at-17.23.03.png 1031w, /images/2011/12/Screen-Shot-2011-12-19-at-17.23.03-300x95.png 300w, /images/2011/12/Screen-Shot-2011-12-19-at-17.23.03-1024x323.png 1024w" sizes="(max-width: 1031px) 100vw, 1031px" />][8]

## Show changes since last backup and start a backup

[<img class="aligncenter size-full wp-image-368" title="Display backups" src="/images/2011/12/Screen-Shot-2011-12-19-at-17.23.19.png" alt="" width="1024" height="353" srcset="/images/2011/12/Screen-Shot-2011-12-19-at-17.23.19.png 1024w, /images/2011/12/Screen-Shot-2011-12-19-at-17.23.19-300x103.png 300w" sizes="(max-width: 1024px) 100vw, 1024px" />][9]

## Managing your data

[<img class="aligncenter size-full wp-image-374" title="Manage user data" src="/images/2011/12/Screen-Shot-2011-12-19-at-17.33.42.png" alt="" width="1020" height="727" srcset="/images/2011/12/Screen-Shot-2011-12-19-at-17.33.42.png 1020w, /images/2011/12/Screen-Shot-2011-12-19-at-17.33.42-300x214.png 300w" sizes="(max-width: 1020px) 100vw, 1020px" />][10]

 [1]: git://github.com/hermanbanken/Ultimate-Single-Sign-On-Enviroment-Installer.git "Repository"
 [2]: /images/2011/12/Screen-Shot-2011-12-19-at-17.22.47.png
 [3]: https://github.com/hermanbanken/ldap-control
 [4]: https://github.com/hermanbanken/ldap-control/issues/2 "issue 2"
 [5]: https://github.com/hermanbanken/ldap-control/issues/1 "issue 1"
 [6]: https://github.com/hermanbanken/ldap-control/issues/3
 [7]: https://github.com/hermanbanken/ldap-control/issues/4
 [8]: /images/2011/12/Screen-Shot-2011-12-19-at-17.23.03.png
 [9]: /images/2011/12/Screen-Shot-2011-12-19-at-17.23.19.png
 [10]: /images/2011/12/Screen-Shot-2011-12-19-at-17.33.42.png