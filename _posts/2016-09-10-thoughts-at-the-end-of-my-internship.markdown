---
layout: post
title: It's over already?! A few thoughts at the end of my internship
date: '2016-09-10 21:49:00'
tags:
- mozilla
- code
- outreachy
---

I can't believe the summer has already ended! Seems like the old adage is true: time flies when you're debugging code.

<div class="img-center">
    
![](/content/images/2016/09/Japanese_Squirrel_edited_version.jpg)
<i>"I hope I have enough acorns in my cache" --sqURL</i>
</div>

I wanted to write a post to acknowledge the end of my internship since that's what's going on timing-wise, but I still have some posts planned for going into some other interesting bugs I worked on this summer (it wasn't all [wrangling Windows 10!](http://blog.katiebroida.com/tag/windows10/)), so stay tuned for that. 

Overall this internship has been huge for helping me improve as a developer, and I highly recommend anyone considering an Outreachy internship (or getting involved in open source in general) to go for it! Working with production code was a great way for me to improve my skills and make some meaningful contributions to software that millions of people use. Thank you [Jared Wein (my mentor)](https://msujaws.wordpress.com/) and the rest of the Firefox QX team for being so welcoming, informative and patient this summer.

I recently put together a presentation for Mozilla on the sort of stuff I was working on and some tips for people look to contribute to improving Firefox's front end, which I've embedded below if you're curious. The other interns' presentations are interesting/worth checking out, but if you want to jump directly to mine (hi Mom!), it starts about 48 minutes in. 

<iframe src="https://air.mozilla.org/outreachy-participant-presentations-summer-2016/video/" width="640" height="380" frameborder="0" allowfullscreen></iframe>

###Optimizing your workflow
Aside from the more high level suggestions I go into during my talk, there were a few technical tools I learned about during my internship that ended up being really helpful time savers. Check them out if you're looking to improve your Firefox contribution workflow.

####[Artifact Builds](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Artifact_builds)
One aspect of working on Firefox that I didn't realize would take a lot of time is rebuilding my local version of the browser to see the effect my changes made. To do a full build on my Mac, it could take 20 minutes or more and on my Windows machine it was more like 45 minutes. Thankfully Mozilla has found a way to speed this up for most developers. Instead of compiling the C++ files yourself, you can instead download them pre-compiled from one of Mozilla's repositories. As long as you're not working on C++ code, this shortcut will save you a lot of downtime waiting for `mach build` to finish. 

[Click here for more details on how to enable artifact builds](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Artifact_builds)

####`mach build faster`
`mach build` is the command used for compiling the entirety of Firefox into an executable file that you then run and check your changes in. However, if you're just making tweaks to the front end code, you can use `mach build faster` to just update the files that don't need to be compiled in a traditional build. This means changes to CSS and JavaScript can be checked more quickly without waiting for the C++ files to compile (or download during an artifact build). 

####Register for notifications from Reviewbot
This is a tool that is a work in progress, but I've found it helpful for staying up-to-date with my reviews. If you enter `!register reviewbot` in a Mozilla IRC chat room (I recommend #reviewbot), you will become registered for pings from Reviewbot anytime the status of your requested reviews change or when a review is requested from you. It's great if you didn't have an eye on your inbox and so wouldn't have seen your Bugzilla emails until later.

I hope you find these tips helpful! If there's any I'm missing, let me know in the comments. 
