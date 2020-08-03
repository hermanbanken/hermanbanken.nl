---
title: HTML5 audio trouble
author: hbanken
type: post
date: 2013-05-05T15:09:21+00:00
excerpt: |
  For a website of a music bind I'm currently developing, I needed a onscreen playlist controlled by javascript.
  
  So halfway during development suddenly the audio started crackling. Whenever I paused and played the audio again and again the crackling started to get worse. To test if this was a HTML5 thing I tried a YouTube video, but that audio also crackled. Googling for 'Chrome audio crackling' gave me some hints, but when I discovered the solution, that was no where near the solutions of the Google-results.
url: /2013/05/05/html5-audio-trouble/
categories:
  - OSX
  - Work
tags:
  - chrome
  - crackling
  - flash
  - html5 audio
  - no sound

---
For a website of a music band that I&#8217;m currently developing, I needed a onscreen playlist controlled by javascript. The features are:

  * Save the songs in the same .playlist as the currently playing audio-tag.
  * Save current seek time of the playing audio-tag as it changes.
  * Save current song in the .playlist, so the index.
  * Restore the playing playlist on page load in a .playlist-player element if this exists.

This way the audio keeps playing when navigating through the website. When the player works, I&#8217;m also going to use AJAX to load the next page and push- and popState to update the URL, for a completely smooth audio experience.

So halfway during development suddenly the audio started crackling. Whenever I paused and played the audio again and again the crackling started to get worse. To test if this was a HTML5 thing I tried a YouTube video, but that audio also crackled. Googling for &#8216;Chrome audio crackling&#8217; gave me some hints, but when I discovered the solution, that was no where near the solutions of the Google-results.

When developing in Chrome I also tried to view the player in an iPad Simulator (from XCode). This was not working and I decided to make things compatible later, and I forgot about having it open. So I restarted Chrome and the crackling was still there, I cleared cache, nothing worked. Then, I remembered the Simulator, closed it, and audio was back normal! The problem is probably with Core Audio being used by both Mobile Safari and Chrome, but I&#8217;m not sure.

You can test this by doing the following:

  1. <span style="line-height: 13px;">Starting Chrome and Mobile Safari (in iOS Simulator) on the same OSX device</span>
  2. Load a website using html5 audio-tags and start playing them on both browsers
  3. Toggle the play pause button a few times in Chrome, audio should become bad.
  4. Close the iOS Simulator and instantly audio goes back to normal in Chrome.

It will not always occur as I discovered doing it the 3rd time, but still: hope this helps anyone.

<div id='gallery-1' class='gallery galleryid-442 gallery-columns-2 gallery-size-thumbnail'>
  <figure class='gallery-item'> 
  
  <div class='gallery-icon landscape'>
    <a href='/images/2013/05/Screen-Shot-2013-05-05-at-17.13.18.png'><img width="150" height="150" src="/images/2013/05/Screen-Shot-2013-05-05-at-17.13.18-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" /></a>
  </div></figure><figure class='gallery-item'> 
  
  <div class='gallery-icon landscape'>
    <a href='/images/2013/05/Screen-Shot-2013-05-05-at-17.12.54.png'><img width="150" height="150" src="/images/2013/05/Screen-Shot-2013-05-05-at-17.12.54-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" /></a>
  </div></figure>
</div>