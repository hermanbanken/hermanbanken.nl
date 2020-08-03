---
title: 'Single-Sign-On OpenLDAP: Part 2 – Schemas'
author: sean
type: post
date: 2011-06-10T16:54:29+00:00
url: /2011/06/10/single-sign-on-openldap-part-2-schemas/
categories:
  - OpenLDAP

---

Guest author: Sean

## Schemas

So what are these schema files anyway?  What do they do and why do we need to add a new one to get authentication with Windoze?  Drop the &#8220;a&#8221; from the end and it will give you a hint.  They are schemes or models or layouts for what our LDAP directory and all of it&#8217;s objects will look like.  They describe every attribute of the directory from top to bottom and put governing rules on what is allowed in the directory.  They don&#8217;t contain any real directory data at all, they only describe what the objects will look like.

There are attributes; which describe each of the individual variable pieces of data, and then there are objectClasses; which describe how all of the attributes are attached and combined so as to describe actual objects.  An example attribute might be called &#8220;name&#8221; and it might be a member of a &#8220;person&#8221; objectClass.  Whenever a &#8220;person&#8221; object is stored in the database, there will always be a spot to put the person&#8217;s &#8220;name&#8221;

What&#8217;s neat about these schemas is that you can describe an object once, and then use that to add meaning to other objects.  For example, you might define a &#8220;posix-user&#8221; class that contains attributes that are relevant to user in a posix environment.  It might have attributes for GID and UID.  Once we describe the &#8220;posix-user&#8221; schema, we can apply that schema to our &#8220;person&#8221; object from before.  Now we have a person that also contains the needed attributes of UID and GID.  If we take this a bit farther and add home directory, shell and password attributes, our &#8220;person&#8221; object  might soon contain all the necessary attributes to be a valid posix user.  Once that happens, suddenly they can log in and galavant around in Linux land.

This is what Herman did with the apple schema, and exactly what we need to do with the samba schema.  To create a &#8220;user&#8221;, I believe the base objectClass we use is &#8220;inetOrgPerson&#8221; which contains all the generic &#8220;person&#8221; info&#8230;address, name, phone numbers, etc.  Then we also give it the &#8220;apple-user&#8221; objectClass (which give it all of the apple specific things), and also now the &#8220;sambaSamAccount&#8221; objectClass (which gives it all of the Windows specific attributes).  When we add the samba schema, it leaves all of the apple info intact and simply sets up a new area for the Samba attributes.  In other words, we are adding an objectClass and all of its attributes, but not messing with whats already there.

Herman&#8217;s installer did include a samba schema file, but after some trial and error, I found out that it was a legacy samba schema file..perhaps for Samba 2.0.  It didn&#8217;t have all of the needed attributes, and many of the attributes it did have used a different naming scheme.  Even if we had had Samba up and running as a PDC, it wouldn&#8217;t have been looking in the right places for the needed attributes.

I&#8217;ll post the schema as soon as I can figure out how to get the system to let me post it.  I don&#8217;t really feel like comparing, but I&#8217;ll bet that what I have matches [this][1].

This was the first part of the puzzle for me&#8230; I&#8217;ll be back with more soon.

~Sean

 [1]: http://www.zytrax.com/books/ldap/ape/samba.html