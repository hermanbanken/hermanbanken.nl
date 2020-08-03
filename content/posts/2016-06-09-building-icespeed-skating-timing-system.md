---
title: Building ice/speed skating timing system
author: hbanken
type: post
date: 2016-06-09T10:57:24+00:00
url: /2016/06/09/building-icespeed-skating-timing-system/
categories:
  - No category
format: aside

---
One of my projects is building my own timing system for speed skating. I have some ideas of how to build this, but to be sure of the feasibility and needed components I decided to [ask some help on reddit/arduino][1]. It is a good description of the project too:

* * *

In ice skating and speed skating they employ Photoelectric Retroreflective Sensors (lasers) to time passing over the start/finish line. As a hobbyist ice/speed skater I want to build such a system so we can organise competitions with our club, and time results precisely. By convention the results are published with hundreds/thousands of a second precision. I want to achieve at least that precision, probably a factor higher to be sure. I&#8217;m looking for some help/guidance building this as I have not completed many circuit builds&#8230;

I have learned that lasers with large range are expensive. Looking at SICK&#8217;s WL12L series at the moment. They are available with a range of 18m for ~100 euros/dollars on Ebay. This component would be by far the most expensive component of course. I&#8217;m looking to build the rest using off the shelf/hobbyist electronics. Example picture of sensor workings:

<img class="size-medium wp-image-548 aligncenter" src="/images/2016/06/msr2-300x86.gif" alt="Retroreflective Sensor" width="300" height="86" /> 

As multiple skaters can ride in series (are on the same track) the output of the system should be a stream of high precision timestamps (relative to some epoch, preferably sync-able by a shared button to both start + finish units). Would need 2 sensors, one per unit: 1 finish line, 1 start line (rolling start).

I&#8217;m wondering if the internal clock of Arduino&#8217;s is sufficiently accurate for timing. There may be some time drift, but a match takes ample minutes, not days. I&#8217;m considering making the timing more accurate using a RTC. Question: does that adjust the clock, say, every second, or does it also increase precision on a sub-second level? Another approach would be using a oscillator with a binary counter IC, but that requires more setup than just using the Arduino. Would it be (measurably) worth it? I&#8217;ve drawn a simplified circuit with oscillator:

<img class="size-medium wp-image-549 aligncenter" src="/images/2016/06/timer-circuit-295x300.jpeg" alt="timer-circuit" width="295" height="300" srcset="/images/2016/06/timer-circuit-295x300.jpeg 295w, /images/2016/06/timer-circuit.jpeg 500w" sizes="(max-width: 295px) 100vw, 295px" /> 

To be ultra portable I want the system to operate over wifi. The operator would use an tablet/iPad to view the results / control the match. Before the match the units could be synced using a coupling button, which would reset the counter IC&#8217;s / Arduino in-memory time offset. To transmit the signal I&#8217;m thinking about using ESP8266&#8217;s (or devices with more powerful antenna) to transmit the stream of times to the central unit. I believe I must use separate boards for the timing chip and the wifi chip, as to not disturb measurements when the WiFi chip is busy sending data. Is that correct? Or would sending data not influence interrupts / timing if using a single ESP instead of Arduino?

TLDR:  
&#8211; is precision of timing on Arduino enough for 100/1000 of a second precise timing? Considering drift over span of 2/3 hours.  
&#8211; does sending data over WiFi disturbs interrupt / timing accuracy?  
&#8211; if so, could this be solved by using 2 distinct chips (communicating over I2C for example) 1 for timing, 1 for WiFi?  
&#8211; any more tips/hints?

 [1]: https://www.reddit.com/r/arduino/comments/4na9bb/advice_for_building_high_speed_sports_timing/