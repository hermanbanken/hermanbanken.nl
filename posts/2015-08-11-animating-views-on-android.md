---
title: Animating views on Android
author: hbanken
type: post
date: 2015-08-11T14:30:21+00:00
url: /2015/08/11/animating-views-on-android/
categories:
  - Coding
tags:
  - software

---
Note to self: whenever you try to animate a view that is either GONE or has height 0 this will not work the first time. The moment the user touches the screen or an UI update is triggered only then will the animation start.

Of course I found [this Stack Overflow question][1] about not starting animations, but the view was defined as being VISIBLE. I also explicitly made the view visible in code &#8211; just to be sure &#8211; but this was not enough. One [response][2] suggested setting the height to something larger than 0. I gave this some thought, but instead of manipulating the height I put in

<pre class="brush: java; title: ; notranslate" title="">view.setMinimumHeight(1)
view.requestLayout</pre>

After some crazy hours investigating threads, creating many Handlers and Runnables I figured to give this height-thingy an extra try. And it worked, finally!

So, whenever you animate the height of a view starting from 0, use:

<pre class="brush: java; title: ; notranslate" title="">Animation animation = new SomeAnimation(view, 500, height);
view.getLayoutParams().height = 1;
view.requestLayout();
view.startAnimation(animation);</pre>

 [1]: http://stackoverflow.com/questions/16238513/animation-not-starting-until-ui-updates-or-touch-event "Animation not starting until UI updates or touch event"
 [2]: http://stackoverflow.com/questions/16238513/animation-not-starting-until-ui-updates-or-touch-event#comment41421815_21033949