---
title: '[OS X] High process numbers'
author: hbanken
type: post
date: 2012-07-12T10:15:39+00:00
url: /2012/07/12/os-x-high-process-numbers/
categories:
  - No category

---
Ever experienced high process numbers (20k+) in Finder? That&#8217;s probably since some process keeps respawning over and over. Some launchAgents fail to start and launchtcl keeps trying to start them. This is filling your hard disk with logs and keeps your hard disk busy writing, preventing it to spin down.

 `12-07-12 11:11:25,732 com.apple.launchd: (org.postgresql.postgres[66258]) Exited with code: 1<br />
12-07-12 11:11:25,732 com.apple.launchd: (org.postgresql.postgres) Throttling respawn: Will start in 10 seconds<br />
12-07-12 11:11:34,034 com.apple.launchd: (com.edb.launchd.postgresql-9.1[66265]) getpwnam("postgres") failed<br />
` 

You should probably fix the error that prevents the agent or daemon to start but thats depending on the kind of agent. In my case I didn&#8217;t need the agents that were spawning. I didn&#8217;t need a PostgreSQL-server and neither the Wiki-server OS X is providing, coming with all kinds of collab* processes. Please proceed only if you don&#8217;t need the agent you are going to remove, permanently!

So, how do you stop these annoying little agents? First determine the process-name. Then lookup the launchctl plist files:  
`<br />
$ sudo launchctl list | grep annoyingAgent<br />
` 

You can unload/remove this plist from launchctl by running:  
`<br />
$ sudo launchctl remove 'com.your.annoying.agent.plist'<br />
` 

Please check now that your computer is still functioning. Do this first, as you can return easily until now. Just replace the &#8216;remove&#8217; with &#8216;load -w&#8217; to re-add the agent.

When you reboot your machine the processes are sometimes coming back and to permanently disable them run:  
`<br />
$ locate 'com.your.annoying.agent.plist' | while read -r line; do sudo mv $line $line.disabled; done;<br />
`