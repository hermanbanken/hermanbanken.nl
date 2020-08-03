---
title: WhatsApp, running on your computer
author: hbanken
type: post
date: 2013-01-13T13:28:25+00:00
url: /2013/01/13/whatsapp-running-on-your-computer/
categories:
  - Apple
  - iPhone
  - OSX

---
[<img class="size-full wp-image-417 alignright" src="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.31-copy.png" alt="WhatsApp in BlueStack App Player" width="220" height="108" />][1]I previously wrote about playing WhatApp through VNC on your Mac, but this wasn&#8217;t useful if you didn&#8217;t have a phone near. Anyhow, there has been some development in the <a title="Github project on WhatsApp" href="https://github.com/venomous0x/WhatsAPI" target="_blank">reverse engineering</a> of Whatsapp and there are also some great emulators available right now, so I would like to introduce to you: Bluestacks running WhatsApp![<img class="aligncenter wp-image-408 size-large" src="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.04-1024x675.png" alt="WhatsApp on Bluestack" width="660" height="435" srcset="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.04-1024x675.png 1024w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.04-300x198.png 300w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.04.png 1180w" sizes="(max-width: 660px) 100vw, 660px" />][2]

**Warning:** you can only have 1 device connected to WhatsApp at the same time. Once you have entered the verification code on your computer, you can&#8217;t use WhatsApp on your phone anymore until you reactivate WhatsApp on your phone. This can only be done 60 minutes after sending the previous verification code. A great way to circumvent this is by using a landline number, since you won&#8217;t be installing WhatsApp on your good old DECT-phone.

So how do you get this running yourself?

  1. Download the following files: 
      * Bluestacks Android Player from <http://www.bluestacks.com/>
      * WhatsApp.apk from <http://www.whatsapp.com/android/>
  2. Install Bluestacks by moving it from the dmg to your Applications folder (Mac) or clicking the installer (Windows).
  3. Start Bluestacks, this can take some time and at least my computer showed 50% CPU loads for a while.
  4. Install WhatsApp in BlueStack by running the follwing in Terminal: 
    <pre>~/Library/BlueStacks\ App\ Player/Runtime/uHD-Adb install \
~/Downloads/WhatsApp.apk</pre>

  5. Start WhatsApp and set it up by entering your Country and Phone number, then wait till the time countdown is up. You will receive a SMS in the mean time, but you can&#8217;t enter this since &#8211; in Android &#8211; WhatsApp will handle this SMS automatically.  After the time is up, WhatsApp will offer to call you. Pick up your phone and enter the code the woman reads on your computer.  
    [<img class="wp-image-409 alignleft" src="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.24.01.png" alt="WhatsApp sending SMS" width="255" height="168" srcset="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.24.01.png 1180w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.24.01-300x198.png 300w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.24.01-1024x675.png 1024w" sizes="(max-width: 255px) 100vw, 255px" />][3] [<img class="wp-image-410 alignleft" title="WhatsApp calling you" src="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.31.00.png" alt="WhatsApp calling you" width="255" height="168" srcset="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.31.00.png 1180w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.31.00-300x198.png 300w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.31.00-1024x675.png 1024w" sizes="(max-width: 255px) 100vw, 255px" />][4]
  6. Enter a name and your good to go. Enjoy!

[<img class="aligncenter wp-image-416 size-large" src="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.21-1024x675.png" alt="WhatsApp contact list" width="660" height="435" srcset="https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.21-1024x675.png 1024w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.21-300x198.png 300w, https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.21.png 1180w" sizes="(max-width: 660px) 100vw, 660px" />][5]

 [1]: https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.31-copy.png
 [2]: https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.04.png
 [3]: https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.24.01.png
 [4]: https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.31.00.png
 [5]: https://hermanbanken.nl/wp-content/uploads/2013/01/Screen-Shot-2013-01-13-at-13.34.21.png