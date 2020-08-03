---
title: '[Tutorial] OpenLDAP server + Mac OSX clients'
author: hbanken
type: post
date: 2011-01-22T01:44:39+00:00
excerpt: Lately I have been busy trying to get my Mac clients to bind to OpenLDAP. I found the learning curve of OpenLDAP very steep. Almost 2 years ago now I did some experimenting with OpenLDAP and quitted because of the lack of tutorials that actually worked at once. I hoped to find an install script that could take away the pain of the configuration. Right now I’ve written one myself and in this article I explain how it works and of course you can download it here too.
url: /2011/01/22/openldap-server-mac-osx-clients/
categories:
  - Apple
  - OpenLDAP
  - OSX
tags:
  - debian
  - ldap
  - linux
  - mac os x
  - open directory
  - openldap
  - server
  - ubuntu

---
<div>
  <p>
    Lately I have been busy trying to get my Mac clients to bind to OpenLDAP. I found the learning curve of OpenLDAP very steep. Almost 2 years ago now I did some experimenting with OpenLDAP and quitted because of the lack of tutorials that actually worked at once. I hoped to find an install script that could take away the pain of the configuration. Right now I’ve written one myself and if you just came for that, go on and skip to the <a href="http://hermanbanken.nl/2011/01/22/openldap-server-mac-osx-clients/#script">download section</a>.
  </p>
  
  <p>
    <!--more-->
  </p><figure id="attachment_205" style="width: 400px" class="wp-caption aligncenter">
  
  <a href="/images/2011/01/Schermafbeelding-2011-01-22-om-04.17.20.png"><img class="size-full wp-image-205 " title="LDAP server Mac Windows Linux clients" src="/images/2011/01/Schermafbeelding-2011-01-22-om-04.17.20.png" alt="LDAP server with Mac, Windows & Linux clients" width="400" height="260" srcset="/images/2011/01/Schermafbeelding-2011-01-22-om-04.17.20.png 666w, /images/2011/01/Schermafbeelding-2011-01-22-om-04.17.20-300x195.png 300w" sizes="(max-width: 400px) 100vw, 400px" /></a><figcaption class="wp-caption-text">LDAP server with Mac, Windows and Linux clients</figcaption></figure> 
  
  <p>
    For people who have never heard of LDAP, Active Directory or Open Directory: you should probably just keep it that way. But if you’re very interested or just want to spend a lot of time frustrating yourself, please read on.
  </p>
  
  <p>
    OpenLDAP is an open source implementation of LDAP, which stands for Lightweight Directory Access Protocol. It basically is a database that is optimized for reading and is mainly used for storing information about clients, users and groups.  A lot of Unix programs are written to support LDAP. For example Contacts, Mail and iCal from Apple can use LDAP to get contact information. Beside client programs, LDAP is also the basis of Directory protocols like Active Directory and Open Directory, protocols respectively developed at Microsoft and Apple.
  </p>
  
  <h1>
    Home Situation & OS X Server
  </h1>
  
  <p>
    <img class="alignright" title="Kernel Panic" src="https://1to10reviews.files.wordpress.com/2007/07/kernel_panic-1p0f.png" alt="Kernel Panic" width="340" height="184" />Since I try to centralize the data and information services in our home I setup a Mac OS X Server about a year ago. All computers in our home could login to this server to get the user home directories and shared data like photo’s, movies and our iTunes library. This way allowing my family to use whatever computer they needed (speed, word-processing), being able to switch computers and not having to buy fast computers only. But after a while, there was one computer short, so I let them login to the server as well. Making that machine a <strong>client and a server at the same time</strong>. That was a <strong>bad choice</strong>! Since I’m kind of a Hackintosh-guy the server was a Hackintosh running Snow Leopard Server. And although Mac OS X is a very stable operating system, sometimes things get screwed up. I think the problem was the unmounting of local NFS drives when logging out of the server, but I’m not sure on that one. Anyway, the server hangs sometimes with a “You need to restart your computer”-kernel-error-message and this being also the server meant all clients losing there connection to the home directories hanging all clients too. The situation had to be changed.
  </p>
  
  <h1>
    OpenLDAP versions
  </h1>
  
  <p>
    Since I’m also into open source and Apple announced that it would drop it’s Xserve line, raising questions about the persistence of OS X Server edition I decided to give up on OS X Server and go for OpenLDAP running on a Ubuntu Server.
  </p>
  
  <p>
    I can’t say there aren’t any good tutorials out there for setting up an environment for Mac, Windows and Linux clients with OpenLDAP, but many of them are deprecated or contain errors. In the footer I’ve added some links to tutorials I followed myself as a reference. Since OpenLDAP 2.3 a configuration backend stored inside LDAP instead of in a file. All tutorials based on older versions sometimes are more specific on how to reach the same goals as I had, but where useless at the start of my LDAP-journey, since they used the old configuration method and my lack of knowledge of LDAP wasn’t enough to convert this to the new way.
  </p>
  
  <p>
    As the time passed and as I was getting more frustrated of reinstalling LDAP over and over again I started to get to know LDAP better and by now I think I can say I understand LDAP quite well. Now the problem shifted to how to get Mac OS X to bind to LDAP and to get Workgroup Manager to edit and add users. I particularly wanted Workgroup Manager to work since it can be used to set some special settings on Mac clients based on the machines physical location, type, current user and group.
  </p>
  
  <h1>
    Schemas
  </h1>
  
  <p>
    The initial installation of OpenLDAP is nothing more than a single command
  </p>
  
  <pre>apt-get install slapd</pre>
  
  <p>
    slapd being the OpenLDAP daemon. But this is of course not enough to get things working. First you need to add the guidelines to how LDAP may store data. With LDAP this is actually completely up to you but when trying to get a program to bind to your database the makers of these programs actually made some guidelines themselves. They are called scheme’s and are located in /etc/ldap/schema but they need to be converted first. Schemas define what kind of sub nodes and attributes nodes in the directory can, need to or may have. Services like Samba and Apple’s Open Directory have there own schemas that can be found on every Mac since OS X in /etc/openldap/schema/apple.schema and /etc/openldap/schema/samba.schema.
  </p>
  
  <p>
    The apple.schema heavily depends on samba.schema and samba.schema depends on other schemas as well. For my working setup only the following schemas were needed:
  </p>
  
  <pre>/etc/ldap/schema/cosine.schema
/etc/ldap/schema/inetorgperson
/etc/ldap/schema/nis.schema
/etc/ldap/schema/misc.schema
/etc/ldap/schema/samba.schema
/etc/ldap/schema/apple.schema</pre>
  
  <p>
    In the OpenLDAP distributions before version 2.3 you only needed to add an include to these .schema files. Since 2.3 you need to add them as LDAP data. Ldif files are the files in which modifications to the directory are stored. So the .schema files need to be converted to .ldif files. This is perfectly well <a title="OpenLDAP Ubuntu" href="https://help.ubuntu.com/10.04/serverguide/C/openldap-server.html" target="_blank">explained by the guys from Ubuntu</a> (search for schema_convert.conf) and I just included the final ldifs for a empty install in my install scripts, so you don’t need to make them yourself.
  </p>
  
  <p>
    Adding these ldifs is easy and doesn’t require complicated commands.
  </p>
  
  <pre>ldapadd -Y EXTERNAL -H ldapi:/// -f cosine.ldif</pre>
  
  <p>
    This command needs to be called for every ldif of the schemas. The –Y tells the LDAP client which authentication protocol should be used and –H tells it which server should be used.
  </p>
  
  <h1>
    ALC
  </h1>
  
  <p>
    Now that LDAP knows to which guidelines it should test its requests to add data it’s time to define to where data will be written and which users can do this. I suggest that you work through the following code yourself if you want to or just read on since it is getting really interesting only after this is done. The following code is added to an empty ldif file and is added the same way as the schemas above. Inside the code are variables ($var) since I’ve taken the code straight from my one-minute-install script. $dn is the root bind path (e.g. dc=example,dc=com) and $rootdn is the path to the root user (e.g. uid=diradmin,ou=users,dc=example,dc=com).
  </p>
  
  <pre># Load dynamic backend modules
dn: cn=module{0},cn=config
objectClass: olcModuleList
cn: module{0}
olcModulePath: /usr/lib/ldap
olcModuleLoad: {0}back_hdb

# Create directory database
dn: olcDatabase={1}hdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcHdbConfig
olcDatabase: {1}hdb
olcDbDirectory: /var/lib/ldap
olcSuffix: $dn
olcRootDN: cn=$usernm,$dn
olcRootPW: $passwd
olcAccess: {0}to attrs=userPassword,shadowLastChange by dn="cn=$usernm,$dn" write by dn="$rootdn" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=$usernm,$dn" write by dn="$rootdn" write by * read
olcLastMod: TRUE
olcDbCheckpoint: 512 30
olcDbConfig: {0}set_cachesize 0 2097152 0
olcDbConfig: {1}set_lk_max_objects 1500
olcDbConfig: {2}set_lk_max_locks 1500
olcDbConfig: {3}set_lk_max_lockers 1500
olcDbIndex: uid,gidNumber,sambasid,uidNumber pres,eq
olcDbIndex: cn,sn,mail,givenName,memberUid pres,eq,approx,sub
olcDbIndex: objectClass eq
olcDbIndex: apple-group-realname,apple-realname pres,eq,approx,sub
olcDbIndex: apple-generateduid,apple-group-memberguid,apple-ownerguid pres,eq

dn: cn=config
changetype: modify
dn: olcDatabase={-1}frontend,cn=config
changetype: modify
delete: olcAccess

dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootDN
olcRootDN: cn=$usernm,cn=config

dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: $passwd</pre>
  
  <h1>
    Population
  </h1>
  
  <p>
    Right now the directory is ready to be filled up with a default structure. Linux and Samba basically only need a group and user path but Mac OS X needs a complete tree for it’s own for it to bind so that will be added, as well as a root user. The ldif code for populating the directory can be found in install_ldap.sh by searching for “Mini DIT” but basically this structure is added
  </p>
  
  <pre>|- com
|- example
| - mounts
| - groups
| - users
|     | - diradmin
| - macosx
|     | - computers
|     | - computer_lists
|     | - config
|     | - &lt;&lt;and a lot more&gt;&gt;</pre>
  
  <p>
    All users are stored in com>example>users and groups are stored in com>example>groups. Apple needs a separate tree to store specific information and especially the config section in macosx is interesting.<br /> <a href="/images/2011/01/Schermafbeelding-2011-01-22-om-04.12.41.png"><img class="aligncenter size-full wp-image-200" title="Populating LDAP, directory tree" src="/images/2011/01/Schermafbeelding-2011-01-22-om-04.12.41.png" alt="Directory tree" height="400" srcset="/images/2011/01/Schermafbeelding-2011-01-22-om-04.12.41.png 265w, /images/2011/01/Schermafbeelding-2011-01-22-om-04.12.41-137x300.png 137w" sizes="(max-width: 265px) 100vw, 265px" /></a>
  </p>
  
  <h1>
    Binding and authentication
  </h1>
  
  <p>
    When trying to bind a Open Directory server to Mac OS X you need Directory Utility, which is a very user friendly but time consuming tool to map attributes in LDAP to attributes of users and groups in the OS. These settings can also be fetched directly from LDAP and because of that I’ve added a macosxodconfig node, which contains all the binding info. When the install script is run this is added to the description attribute of this node and when you add the server to Directory Utility you should choose “Fetch from server” or something like this and specify ou=macosxodconfig,cn=config,ou=macosx,dc=example,dc=com when you asked for the location of the configuration. Most of the information about binding I got from a old tutorial from <a href="http://www.siriusit.co.uk/resources/siriuslibrary/osx-and-openldap-taming-leopard" target="_blank">Sirius</a>.
  </p>
  
  <p>
    <a href="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.26.png"><img class="aligncenter size-medium wp-image-197" title="Adding LDAP server in Directory Utility" src="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.26.png" alt="Adding LDAP server in Directory Utility: select Fetch from server" srcset="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.26.png 643w, /images/2011/01/Schermafbeelding-2011-01-22-om-04.05.26-300x206.png 300w" sizes="(max-width: 643px) 100vw, 643px" /></a>
  </p>
  
  <p>
    <a href="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.42.png"><img class="aligncenter size-full wp-image-198" title="Set search base" src="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.42.png" alt="Set search base to ou=macosxodconfig,cn=config,ou=macosx,dc=example,dc=com" width="669" height="270" srcset="/images/2011/01/Schermafbeelding-2011-01-22-om-04.05.42.png 669w, /images/2011/01/Schermafbeelding-2011-01-22-om-04.05.42-300x121.png 300w" sizes="(max-width: 669px) 100vw, 669px" /></a>
  </p>
  
  <p>
    When the configuration is read from the server you’re actually ready to go. Which means you can login with accounts in LDAP and use Workgroup Manager. To check if things are working you should first look in the System Preferences > Accounts > Login options > Network account servers > Edit to see if the light is green. If it’s not, please check if you can ping your server, there is probably a connection issue.
  </p>
  
  <p>
    If the light is green you should move to Terminal and try the command `id <username>` where username is a user that only exists in LDAP. You should get a response like:
  </p>
  
  <pre>uid=1000(diradmin) gid=80(admin) groups=80(admin),402(com.apple.sharepoint.group.1),62(netaccounts),12(everyone),404(com.apple.sharepoint.group.3),401(com.apple.access_screensharing),403(com.apple.sharepoint.group.2)</pre>
  
  <p>
    Then you should check if you can change the user to this user by typing `su <username>` and you should now see a new bash prompt. Congratulations, you just logged in to your new LDAP server from Mac OS X.
  </p>
  
  <h1 id="script">
    Using the script
  </h1>
  
  <p>
    The script is written for a freshly installed Ubuntu 10.04 LTS setup without slapd (since the script will take care of that) and needs to be executed as root or with sudo rights as it needs to use apt-get. So:
  </p>
  
  <pre>sudo ./install.bash</pre>
  
  <p>
    My script first asks for several variables that are needed for the setup and then executes another script that does the installing. First we want to now a username for the Directory Administrator that will be the person that can change passwords and add records, most often this username is “diradmin” and you should probably choose a very long and complicated password for this user.
  </p>
  
  <p>
    For installing LDAP you also need a base domain name, a FQDN, Fully Qualified Domain Name, which should look like dc=example,dc=com but can also be longer or shorter, containing at least one dc. You should really think about this as you can’t change this later on and it will be omnipresent. The domain name doesn’t needs to be legit (e.g. end on dc=com or dc=co,dc=uk) but you should make sure nobody else can register the domain if it is legit.
  </p>
  
  <p>
    After you agree to the information the server will be automatically setup. Only thing left to do is to add the server to the configuration of the clients using System Preferences > Account > Login options > Network account servers > Edit.
  </p>
  
  <p>
    If you like my post, my script or have any comments or suggestions, feel free to leave a <a href="#reply-title">comment</a> below.
  </p>
  
  <h1>
    Download
  </h1>
  
  <p>
    The <a title="LDAP installatie scripts" href="/images/2012/04/OpenLDAP.zip" target="_blank"><span style="color: #008000;">scripts can be downloaded here</span></a>. The zip contains 8 files and 1 folder:
  </p>
  
  <pre>install.bash
install_scripts
 - install_ldap.sh
 - apple.ldif
 - cosine.ldif
 - inetorgperson.ldif
 - misc.ldif
 - nis.ldif
 - samba.ldif</pre>
  
  <h1>
    Resources
  </h1>
  
  <p>
    A great tutorial on adding Mac OS X support to OpenLDAP:<br /> <a href="http://www.siriusit.co.uk/resources/siriuslibrary/osx-and-openldap-taming-leopard">Sirius IT: OS X and taming the Leopard</a><br /> An install script for LDAP without support for longer DN&#8217;s and no support for Mac OS X. Used as reference.<br /> <a href="http://www.ghacks.net/2010/08/31/set-up-your-ldap-server-on-ubuntu-10-04/">GHacks: Set up your LDAP server on ubuntu</a><br /> Used this to find out which indexes to add on the directory for fast Workgroup Manager usage.<br /> <a href="http://www.netmojo.ca/2008/04/24/integrating-leopard-server-with-unix-ldap-part-3/">Netmojo: Integrating Leopard server with Unix</a>
  </p>
</div>