---
title: Want a free/open-source Apple Home Server?
author: hbanken
type: post
date: 2011-03-27T19:00:36+00:00
url: /2011/03/27/want-a-freeopen-source-apple-home-server/
categories:
  - Apple
  - OpenLDAP
  - OSX

---
<img class="alignright size-thumbnail wp-image-306" src="/images/2011/03/science_apple_tree_groot-150x150.png" style="float:right" alt="" width="150" height="150" srcset="/images/2011/03/science_apple_tree_groot-150x150.png 150w, /images/2011/03/science_apple_tree_groot.png 222w" sizes="(max-width: 150px) 100vw, 150px" />When Apple introduced the Mac Mini server version nobody knew. When Apple discontinued the XServe people started thinking. When the changes for OS X Lion became public and OS X Server was merged into the client-version, everybody should have known. When the [rumors came that Apple will stop using Samba][1] and implement their own SMB2 server/client, I knew: Apple wants to give the world an alternative to Windows Home Server (WHS).

**Whats in for me?**<!--more-->

  
This means you can save all your files on a central point in your home without leaving your Apple-fanboy-philosophy by buying a pc server. All seamlessly integrated with bonjour. And now you can, when using Bootcamp or Parallels, also link Windows clients.

**Did anybody said free?**  
Back to the open-source thing from the title, since that probably was why you came here: when I started my [Linux-box-acting-as-a-OSX-Server-box-project][2] I thought Apple would always continue to use Samba and therefore use only technologies that have open-source implementations (Bonjour -> Avahi, AFP -> Netatalk, etc). This way I could mimic all features of the Apple Server, for free.

But Samba currently only features NT-domaincontroller (for older systems < Vista) and the Apple implementation will support Active Directory (aka or related to SMB2). So when Lion will we introduced it won't be possible to do every thing OS X does with Linux. I think this still is a good thing since the Samba team will be pushed to work on and release Samba 4. This new version will add support for Active Directory and will solve the binding-problems you currently can experience with Vista/Win7. When Samba 4 will lose it's beta mark I will update my automagically-install-script for Ubuntu making installing Ubuntu and mimicking OSX almost a one-click, Apple-like experience.

 [1]: http://www.appleinsider.com/articles/11/03/23/inside_mac_os_x_10_7_lion_server_apple_replaces_samba_for_windows_networking_services.html
 [2]: http://hermanbanken.nl/2011/01/22/openldap-server-mac-osx-clients/