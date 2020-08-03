---
title: NewRelic causing OutOfMemoryError in Play Framework app
author: hbanken
type: post
date: 2015-03-01T14:42:22+00:00
url: /2015/03/01/newrelic-causing-outofmemoryerror/
categories:
  - Coding
  - Work
tags:
  - newrelic
  - playframework
  - scala
  - zipoutputstream

---
Today I disabled the NewRelic JVM agent on one of my projects. While the Play Framework server was outputting a [ZipOutputStream][1] to a client, the NewRelic agent would for some reason gather massive amounts of data and cause the JVM to Garbage Collect continuously until the app became unresponsive, and finally crashed:

<pre class="brush: plain; title: ; notranslate" title="">Uncaught error from thread [play-akka.actor.default-dispatcher-33] shutting down JVM since 'akka.jvm-exit-on-fatal-error' is enabled for ActorSystem[play]
java.lang.OutOfMemoryError: GC overhead limit exceeded
Uncaught error from thread [play-scheduler-1] shutting down JVM since 'akka.jvm-exit-on-fatal-error' is enabled for ActorSystem[play]
java.lang.OutOfMemoryError: GC overhead limit exceeded
[ERROR] [02/27/2015 14:45:10.002] [application-scheduler-1] [ActorSystem(application)] exception on LARS? timer thread
java.lang.OutOfMemoryError: GC overhead limit exceeded</pre>

Since september I had been using NewRelic&#8217;s v3.10.0 agent. The specific zip streaming that was causing the issue was a feature supposed to be used starting Februari 27, but of course the feature was tested before. Both locally and in production the feature seemed to work, for smaller amounts of files. However in production the typical amount of files in the zip would be more than 1000 each of them ranging from several KB to 0.5MB. As soon as we discovered the issues we started delving into what could have caused the symptoms: a server that would not handle anymore requests, using 100% CPU and it maximum allowed memory size (-Xmx1024m). We did [refactor the complete logic responsible for serving the zip][2]&#8216;s, but to no avail. Locally the new method seemed better: now the zipping would not continue to use resources after the request would be closed prematurely. We also wrote a test that would simulate zipping random files, this also worked, locally.

Locally however, no NewRelic was installed. How could a service responsible for showing problems ever be the cause of the problems, we thought.

It now being past Februari 27, the zip streaming had been enabled for our users. We saw an immediate increase in downtime: the server would hang, we would ironically get an e-mail from NewRelic, and we would restart the server. Of course the logs directed us to the culprit: the initiation of a zip stream was always the last action before the downtime.

This weekend I decided to investigate, and learned all new tricks. Having never done a Heap Dump before this felt quite tricky at first, but in the end it was very easy:

> ssh server &#8220;ps x | grep play&#8221;  
> ssh server &#8220;sudo -u play jmap -dump:file=/dump.hprof <processid>&#8221;  
> scp server:/dump.hprof  
> # Open dump with [Eclipse&#8217;s MemoryAnalyser][3]

Eclipse Memory Analyser is a free tool that is very easy to use. It starts importing your dump file (which is way faster in Run in Background mode!) and then shows really helpful statistics:

<div id='gallery-2' class='gallery galleryid-484 gallery-columns-1 gallery-size-thumbnail'>
  <figure class='gallery-item'> 
  
  <div class='gallery-icon landscape'>
    <a href='http://hermanbanken.nl/wp-content/uploads/2015/03/dump1.png'><img width="150" height="150" src="http://hermanbanken.nl/wp-content/uploads/2015/03/dump1-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-2-493" /></a>
  </div><figcaption class='wp-caption-text gallery-caption' id='gallery-2-493'> Prime suspect: com.newrelic.agent.Transaction </figcaption></figure>
</div>

After seeing this analysis I was baffled: how could this be? So I searched and found [this thread][4] on the New Relic forum from October 2014. More people had this issue! In December they [released version 3.12.1][5] which has the following release notes:

> ### Improvements
> 
>   * Play 2 async activity is no longer tracked when transaction is ignored.
>   * Reduced GC overhead when monitoring Play 2 applications.
>   * Reduced memory usage when inspecting slowest SQL statements
> 
> [..]

&#8230; they said. So I updated to a new version of the agent. It did not work. The server would still hang caused by the many Transactions stored in the queue. New statistics:

<div id='gallery-3' class='gallery galleryid-484 gallery-columns-2 gallery-size-thumbnail'>
  <figure class='gallery-item'> 
  
  <div class='gallery-icon portrait'>
    <a href='http://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.46.png'><img width="150" height="150" src="http://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.46-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-3-491" /></a>
  </div><figcaption class='wp-caption-text gallery-caption' id='gallery-3-491'> Prime suspect: com.newrelic.agent.Transaction </figcaption></figure><figure class='gallery-item'> 
  
  <div class='gallery-icon landscape'>
    <a href='http://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.58.png'><img width="150" height="150" src="http://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.58-150x150.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-3-492" srcset="https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.58-150x150.png 150w, https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.58-300x300.png 300w, https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.58.58.png 850w" sizes="(max-width: 150px) 100vw, 150px" /></a>
  </div><figcaption class='wp-caption-text gallery-caption' id='gallery-3-492'> Detail </figcaption></figure>
</div>

Too bad that the update did not fix the issue. I hope that NewRelic can fix the issue in the future, as I really liked the assurance that you get mails when your server is down, but also being able to drill down into performance issues. By the way: besides the issue still being there, also the apps performance decreased when using the updated agent:<figure id="attachment_496" style="width: 499px" class="wp-caption aligncenter">

[<img class="wp-image-496" src="https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.59.54.png" alt="Web request performance before and after updating from New Relic agent 3.10 to 3.14" width="499" height="229" srcset="https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.59.54.png 767w, https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.59.54-300x138.png 300w" sizes="(max-width: 499px) 100vw, 499px" />][6]<figcaption class="wp-caption-text">Performance decrease from going from New Relic agent 3.10 to 3.14</figcaption></figure> 

&nbsp;

 [1]: http://blog.bradleywagner.org/2013/08/tips-and-pitfalls-when-using-javas-zipoutputstream/ "Common pitfalls when using ZipOutputStream for massive amounts of files"
 [2]: https://gist.github.com/hermanbanken/3af6bfea6e2ec462be52 "refactored code for ZIP streaming using Play Framework's Enumerator/Iteratee library"
 [3]: https://projects.eclipse.org/projects/tools.mat/downloads "Eclipse Memory Analyzer"
 [4]: discuss.newrelic.com/t/outofmemory-error-because-of-new-relic/10340/6
 [5]: https://docs.newrelic.com/docs/release-notes/agent-release-notes/java-release-notes#Java-Agent-3.12.1 "NewRelic release notes"
 [6]: https://hermanbanken.nl/wp-content/uploads/2015/03/Screen-Shot-2015-03-01-at-14.59.54.png