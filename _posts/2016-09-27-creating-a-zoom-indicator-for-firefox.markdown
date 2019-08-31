---
layout: post
title: Creating a Zoom Indicator for Firefox
date: '2016-09-27 02:15:39'
tags:
- mozilla
- code
---

Don't be fooled by my previous posts, I didn't just work on [Windows 10 bugs](http://blog.katiebroida.com/tag/windows10/)  during my Firefox internship! I also created a button that shows a page's zoom level in Firefox if it's not set to the default (100%). In this post I'll walk through why Firefox needs a zoom indicator and some highlights of the build process. 

![Enhance](/assets/images/2016/07/enhance.png)
<i>"Enhance"</i>

### Why have a zoom indicator?
To add yet another button to the UI, there should be a good reason for it. In this case, Firefox needs a zoom indicator because there are a few ways to accidentally zoom into your browser screen without realizing it (trackpad gestures, hitting "control" while using a mouse's scroll wheel, keyboard hot keys). Previously there was no way to see at a glance what your zoom level in Firefox was. This made is likely that a Firefox user could be zoomed in on a certain site and not realize they weren't seeing the content displayed as intended.  

[When we were planning on how to display the page's zoom](https://bugzilla.mozilla.org/show_bug.cgi?id=565718), it was helpful to take a look at how other browsers were handling this issue. Chrome currently has a fairly large display of a page's zoom when you first update it, but it soon disappears after the zoom level change: 

![Chrome's zoom feature](/assets/images/2016/09/chrome-zoom.gif)

As for Safari and Internet Explorer, there currently is no way to see in the UI what your current zoom level is unless you open up a menu option or two (unless I'm missing something). We decided to go similar route as Chrome and have an indicator in the URL bar, but we would have it stay visible when the zoom isn't at 100% instead of letting it fade away. 

### The Fix
One cool thing about creating this button from scratch was that I was able to see how a project evolves from a mockup to an implemented feature. Initially we had planned on having some pretty fancy animations:

![](/assets/images/2016/09/zoomMockup1.gif)

While the sliding "x" icon was visually appealing and made it more obvious that this zoom button was clickable, in practice its sliding out made the history dropdown button (an upside triangle to the left of the zoom button) harder to click because it jumped around when the zoom button animation expanded and retracted. Also because of how [Firefox toolbar buttons have some images bound to them](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XUL/Tutorial/Introduction_to_XBL), I couldn't add the icon image to the right of the button text so it could instead slide out to the opposite side. 

To create the initial implementation of the zoom button, we ended up simplifying the animations and removing a lot of extra styling. This is what it looked like when I had finished and it landed in Nightly (the beta version of Firefox) for the first time:

![](/assets/images/2016/09/zoomActual.gif)

As you can see, the zoom button is visible when the browser's zoom level is something other than 100%. You can even reset the zoom back to its default level by clicking the button in the URL bar. The button's styling is still being tweaked to make it more appealing/obvious you can click it, but the basic functionality is there!

### Test it

I learned a ton working on this particular bug, but I think the biggest thing that surprised me was how much Firefox's appearance varies across operating systems. I didn't understand my reviewer's feedback on the button's appearance at first because he was taking a look at it using Linux and I was using OS X. Some advice I would give to anyone adding to Firefox's UI is to take a look at your changes on other operating systems before submitting them for review. It can save you some back and forth on at least the more obvious visual issues. 
