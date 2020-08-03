---
title: iPad 2/JailbreakMe 3.0 FAQ
author: veeence
type: post
date: 2011-07-02T20:37:13+00:00
url: /2011/07/02/ipad-2jailbreakme-3-0-faq/
categories:
  - No category
tags:
  - faq
  - ipad
  - jailbreak
  - jailbreak me
  - shsh blobs

---

Guest author: veeence

[<img class="aligncenter size-full wp-image-361" title="iPad 2. Finally, Jailbroken." src="https://hermanbanken.nl/wp-content/uploads/2011/07/JailbreakMe-Purple-Screen1.jpg" alt="" width="620" height="233" srcset="https://hermanbanken.nl/wp-content/uploads/2011/07/JailbreakMe-Purple-Screen1.jpg 620w, https://hermanbanken.nl/wp-content/uploads/2011/07/JailbreakMe-Purple-Screen1-300x113.jpg 300w" sizes="(max-width: 620px) 100vw, 620px" />][1]Since there is a lot of confusion out there, and since I’m repeating myself all the time (which I do not really like), I made this little write up of questions that are continuously being asked (my personal FAQ). Please note that this is a global explanation. Don’t try to argue with me on specific details.  
This FAQ has been written by [@veeence][2] and is being powered by [@hermanbanken][3].

**_<span style="text-decoration: underline;"><!--more-->I will update this FAQ, so be sure to check back! If you have more questions, leave them in the comments and I will try to answer them in updates of this FAQ.</span>_**

**Why do I need to save my SHSH blobs??**

Every time you restore an iPhone, iPad or iPod touch (arm7 versions), iTunes/iDevice contacts Apple and asks if it is OK to restore to this firmware version. Apple verifies whether the version you are restoring to, is the latest one. If so, Apple will send the SHSH and the restore can continue, if not, Apple will refuse to send the SHSH and you will not be able to restore to that specific firmware version. Saving your SHSH will make sure you will be able to restore to that specific firmware version you have the SHSH blobs from (on 4.x at least). You will be able to bypass the &#8220;asking Apple for SHSH blobs&#8221;, bacause you already have them for that firmware version.

So, in this case you can jailbreak 4.3.3 with JailbreakMe 3.0 (soon). You want to save your SHSH blobs for 4.3.3 so you can always restore to 4.3.3 if something happens and you have to restore to factory settings when Apple pushes 4.3.4 that will fix the JailbreakMe 3.0 exploit. If you \*don&#8217;t\* safe them, you will end up having to restore to 4.3.4 and you will not be able to jailbreak until a new exploit is found, so save them!

**Ok, how do I save my SHSH blobs?**

Get TinyUmbrella ([link][4]), go to the &#8220;Advance&#8221; tab and uncheck &#8220;Request SHSH From Cydia&#8221;, make sure all other apps that use port 80 are closed, and hit &#8220;Save SHSH&#8221;. Check in the logs whether your blobs for 4.3.3 have been saved. All the other firmware versions are not important anymore, you are indeed already too late, but that doesn&#8217;t really matter as long as you have them for 4.3.3.

**I have an iPad 2 3G, what&#8217;s the problem?**

Your iPad has a modem/baseband that runs a different firmware from iOS. It is being signed as well, but you cannot save the SHSH blobs for it. That is because each time you ask for a verification (when restoring), Apple will create &#8220;random&#8221; SHSH blobs, so you can&#8217;t actually reuse the SHSH blobs. This is a problem, because iTunes will not be able to sign the baseband, the bootrom will refuse to boot and give you a 1004 error in iTunes. Since all previous iDevices had a bootrom exploit, you could use TinyUmbrella and &#8220;Kick Device Out Of Recovery&#8221; to boot. This is not possible on the iPad 2 since it has the A5 bootrom which is not yet exploited. You will be stuck in Recovery Mode.

**Will the iPad 2 WiFi have this too?**

No. It doesn&#8217;t have a baseband and it will happily restore with you **BACKED UP** SHSH blobs.

**So what? We&#8217;ll never restore!**

Trust me, there are moment that you&#8217;re going to have to restore your device. Especially when you jailbreak it and install crap.

**Can I upgrade to 4.3.3?**

Yes, JailbreakMe 3.0 will jailbreak all devices (arm7) on all versions of 4.3.x.

**So what should we do now?**

&#8211; Make sure you have you 4.3.3 SHSH&#8217;s backed up  
&#8211; You can update to 4.3.3 if you want  
&#8211; Wait for comex to release the official JailbreakMe 3.0  
&#8211; Stay away from 4.3.4 as far as you can

**If I understand this well, does that mean that the SHSH blobs for the iPad 2 3G version are useless?**

Yes and no. Right now, if Apple stops signing 4.3.3, you won&#8217;t be able to downgrade/restore successfully to 4.3.3. But they might find a work around for this baseband problem, and then your SHSH blobs will be useful again. **Save them anyway!**

&nbsp;

&nbsp;

 [1]: https://hermanbanken.nl/wp-content/uploads/2011/07/JailbreakMe-Purple-Screen1.jpg
 [2]: http://twitter.com/#!/veeence
 [3]: http://twitter.com/#!/hermanbanken
 [4]: http://thefirmwareumbrella.blogspot.com/