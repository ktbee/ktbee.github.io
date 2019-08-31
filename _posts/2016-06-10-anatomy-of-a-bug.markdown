---
layout: post
title: Anatomy of a bug
date: '2016-06-10 14:40:43'
tags:
- code
- mozilla
---

![Anatomy of a bug](/assets/images/2016/06/anatomy-of-a-bug-web.jpg)

I'm currently doing [an internship with Mozilla fixing UI bugs](http://blog.katiebroida.com/getting-started-contributing-to-firefox/) and towards the beginning of my work, my mentor wrote up a [helpful blog post on how he went about tracking down a bug](https://msujaws.wordpress.com/). Reading his post got me thinking about my own troubleshooting process. When I'm working on a bug, it sometimes feels like I'm just checking and trying things until they work. However after thinking about what these UI issues had in common, I realized there are a few questions I try to answer each time:

### How is this feature supposed to work?
Though I'm writing this post in the context of my internship, this isn't the first time I've worked with software bugs. Before this internship I was a support engineer for a SaaS company, and sometimes I found myself running into a bug while working with a customer. While stressful, I found this customer-facing debugging process has been helpful for quickly diagnosing bugs. When I was talking to a customer, it was crucial (and sometimes challenging) to understand what the expected behavior was. Understanding the context behind how customer was using the software could make all the difference in being able to reproduce an issue or not (and no re-pro meant the case was dead in the water and the customer experience wasn't great). 

Though I'm not working with customers now (and I'm definitely not debugging live on the phone), I still spend a fair amount of time making sure I understand how the software is *supposed* to work. Knowing this also helps answer my next question ...

### How is it broken? 
This question is probably an obvious one to ask, but it can take a surprising amount of digging to answer fully. I'll use [a recent bug I fixed as an example](https://bugzilla.mozilla.org/show_bug.cgi?id=1260595). In this issue, themes in Firefox's [Customize Mode](https://support.mozilla.org/en-US/kb/customize-firefox-controls-buttons-and-toolbars) would be listed in their menu multiple times if the menu wasn't closed correctly. Here's a screenshot from the bug report:

![Duplicated themes listing](/assets/images/2016/06/All-items-in-Themes-panel-are-duplicated.png)

This theme issue was sort of funny because you had to go through some very specific steps to repro it. To duplicate the themes, you needed to open the menu and then close the tab with the menu open. When you go back to Customize Mode, you would find the themes had been duplicated when you re-opened the themes menu. However if you closed them menu before closing the tab, the themes wouldn't be duplicated. This made me think that some event wasn't firing as it was intended, either it wasn't firing at all or was firing too many times. From looking at the code on the "Themes" menu button, I understood how it was supposed to work. The menu items were supposed to be cleared when the menu closed, and then re-populated when it was opened. This made me think the menu clearing "onpopuphidden" event wasn't firing if the menu wasn't explicitly closed (leaving it open and closing the tab apparently didn't count as the popup menu closing for the event). 

To make sure the menu was cleared even without the onpopuphidden event, I added a call to the menu clearing function at the beginning of the function that populates the menu. This way no matter what, the menu will be cleared whenever it is populated. Problem solved! I patted myself on the back and submitted a patch. However, another contributor pointed out that I hadn't quite gotten to the bottom of how this thing was broken:

> #### [arni2033](https://bugzilla.mozilla.org/show_bug.cgi?id=1260595#c10) 
>Why doesn't 'popuphidden' event fire? Well, _that_ is the real bug. And modifying functions is a hack. However, such hack is still a very good thing if somebody will also file the underlying bug. If somebody agrees with me and has a better understanding of XUL events, please file it (or OK, make it clear for me that I'll need to debug it myself)

I thought it was interesting that even though I had managed to fix the problem, I still didn't understand the underlying behavior 100%. Lesson learned, you can always go deeper into the rabbit hole.

![swarmmmmm](/assets/images/2016/06/Animal-Insect-Bees-Medieval-Swarm.jpg)

### Does any of it work correctly?
I find myself asking this question both when I'm trying to understand how a feature works and what possible solutions there are for fixing the bug. For example in the themes menu bug, knowing that the menu clearing function worked correctly in some circumstances helped me realize the reason why it didn't work in others. 

[For a bug I'm currently working on](https://bugzilla.mozilla.org/show_bug.cgi?id=565718), I based my changes on what already existed in the Mozilla code base to extend an existing feature. Currently Firefox doesn't have a way in the UI to show a user that their browser window is zoomed in on a page unless you purposefully add the zoom widget from Customize Mode to your browser toolbar. My task was to add a small zoom indicator in the URL bar when the user doesn't have their zoom at the default level (100%). To get started, I looked at other tools that use the URL bar to see how I should structure my code addition. Looking at Reader Mode, I decided to add my code as a [module](https://developer.mozilla.org/en-US/docs/Mozilla/JavaScript_code_modules/Using). I then looked at how the current zoom buttons worked to take advantage of the zoom functions that already existed. 

That's it, that's the secret sauce. I hope breaking down this process is helpful. If you have your own tricks of the trade, let me know in the comments!

![Caterpillar image](/assets/images/2016/06/caterpillar-vintage-illustration.jpg)
