---
title: Code Reviews
author: hbanken
type: post
date: 2013-04-19T19:21:41+00:00
url: /2013/04/19/code-reviews/
categories:
  - TU Delft
  - Work
tags:
  - development
  - programming
  - Zef.me

---
Scrolling through my twitter timeline I read a tweet from Zef Hemel and found myself reading his blog the complete afternoon. One post that catched my attention was entitled &#8220;Never Commit to Master&#8221;.

<!--more-->

The way we develop code has rapidly changed in the last few years. At least the way I develop code has changed, but I see these changes throughout each development team I encounter. 

In the beginning there was no version control, then came centralized version control systems like SVN, and now we have decentralized asynchronous version control systems like Git and Mercurial. These changes made it easier for teams to work together on more related code.

But what&#8217;s next? We&#8217;ve seen things like ticket systems, scrum, pair programming and others. One nice thing to implement in your team is Code Reviews. Systems like Github make it easy to implement Code Reviews as Zef (zef.me) explains:

> When the feature has been completed from a code and test perspective, it is time to issue a pull request with the main branch. GitHub and Bitbucket both provide very nice UIs for this purpose these days. In the pull request description, clearly describe what the feature/bug fix is supposed to do and reference any related issue. You can use the @username notation in the description field to assign the review to one or more people, who will receive an e-mail about the new pull request. 

Following the philosophy of continuous integration fans like Zef and myself the main branch, &#8220;master&#8221; or &#8220;default&#8221;, should always be stable. Committing to this branch is dangerous and should never be done. A commit is a one-men&#8217;s-work and should never be held for stable without review, as Zef wants to make clear. 

> So, the rules are pretty simple; there’s only one:
> 
> **Never commit directly to the main branch.**
> 
> Well, unless you have to fix this one litt… No! Never. Commit. To. The. Main. Branch. Not for big whopping new features, not for a small bugfix.

I&#8217;m not sure if I can force myself to use this standard. Not only do I have to many ideas I make proof of concepts for on my one, but also &#8211; when working in a team &#8211; everyone you work with has to agree. And sometimes people are just lazy, or work at times no Review can be done by people working at day time, and the fix really needs to be pushed.

But one thing is sure: having two pair of eyes look at some code will keep at least some bugs out of the main branch.

[Read Zefs article at his blog][1].

 [1]: http://zef.me/4731/never-commit-to-master