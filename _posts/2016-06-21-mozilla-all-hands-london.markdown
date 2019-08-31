---
layout: post
title: 'T.A.R.D.I.S.: Talking About Recent Discoveries I''ve Seen'
date: '2016-06-21 20:58:00'
tags:
- mozilla
- code
---

### My experience at Mozilla's All Hands meeting in London

![Tardis](/assets/images/2016/06/tardis__x-large.jpg)

Last week I attended Mozilla's All Hands meeting and visited London. They have [these meetings](https://wiki.mozilla.org/All_Hands) twice a year to give everyone in the organization a chance to meet each other face to face, with some people traveling from as far away as New Zealand and Taiwan. Mozilla is such a large, distributed organization, that I think everyone looks forward to hashing things out more quickly during these meetings and further developing relationships among humans. I was also really into the Doctor Who theme they had going on. I think the Mozilla event planners knew their audience when they were deciding how to celebrate the London location. 

![Dr Who hashtags](https://media.giphy.com/media/FLUkjdQ1u6COY/giphy.gif)

I was impressed and really appreciative that Mozilla flies out everyone in the organization, including interns. Otherwise I'm not sure I would have had a chance to meet my mentor in person, and I've found that something essential seems to be missing from purely IRC or email interactions. We were able to wrap up a bug fix that I had been struggling with for a long time in just 20 minutes or so (the issue ended up being some kind of cache on my computer. It always seems to be a cache somehow :/ ). 

### Track me if you can 
Aside from meeting my mentor and other Outreachy interns, I also got to meet other members of the Firefox team and explore other parts of the Mozilla organization. I ended up in a lot of meeting sessions that had to do with security. In particular I was really interested in one regarding ad companies' tactics for "fingerprinting" your device online to track your browsing habits across sites and in spite of cookie clearing. They manage to find some small differences between individual devices and use this to create a unique identifier and build a profile of the user. The articles below go into this in more details. Pretty interesting stuff:

[http://spectrum.ieee.org/computing/software/browser-fingerprinting-and-the-onlinetracking-arms-race](http://spectrum.ieee.org/computing/software/browser-fingerprinting-and-the-onlinetracking-arms-race)

[https://www.propublica.org/article/meet-the-online-tracking-device-that-is-virtually-impossible-to-block](https://www.propublica.org/article/meet-the-online-tracking-device-that-is-virtually-impossible-to-block)

### Open hardware
Another topic I got excited about was open hardware and some of Mozilla's explorations in IoT projects. The founders of [WeVolver](https://www.wevolver.com/home/), Bram Geenen and Richard Hulskes, gave a talk on the open hardware movement and some of the challenges they're facing in growing the community. Because open hardware is about making physical objects, they have some hurdles in creating an open source network that software doesn't. These are the stumbling blocks they described:

#### Version control software
Usually when people think of open source software, they think of GitHub. While GitHub is great for posting projects, its real power comes from allowing people to fork (ie clone or copy) projects, make changes, and have those changes saved and made available to the original project owner. This allows projects to improve and mutate through community contributions. For hardware, this system is hard to create because there's a lot of different file types for a project's different types of media (images, text for assembly instructions, 3d files) and even within the same type of media (like 3d files), there are differing files types and systems for describing an object. It sounds like a real challenge, but if the WeVolver folks can create version control for hardware projects, I can see it really taking off. 

#### Cost of components
Software is easy to get started hacking on because the cost to start is essentially zero (assuming you already have a computer and an internet connection). With hardware, you need to invest some money up front to buy parts and/or access to tools. This is a tough problem, though [some components seem to be getting cheaper all the time](https://www.engadget.com/2016/05/16/raspberry-pi-zero-camera-module/). Hopefully the increasing popularity of makerspaces will help close the gap with tool access as well. 

![Dr Who saying "well"](https://media.giphy.com/media/12WPr8omNjZdle/giphy.gif)
*There's always something to figure out*

### IoT
Another area I explored was connected devices. I had a chance to sit in on some of Mozilla's IoT discussions and demos. It seems like they're still in the early stages of developing and exploring IoT projects, but one I got excited about is FlyWeb. FlyWeb uses existing web technologies to allow phones to discover web apps that are physically nearby and interact with them. I have a grocery list/tracking app I've had in mind for a while that could work well with FlyWeb. I think I'll give it a shot as one of my upcoming projects. To read more about FlyWeb, check out the article below (also has good info on other Mozilla connected devices endeavors) and the Mozilla wiki page:

[Firefox OS Pivots into Connected Devices](https://www.sitepoint.com/the-firefox-os-pivot-into-connected-devices/)

[https://wiki.mozilla.org/FlyWeb](https://wiki.mozilla.org/FlyWeb)

Overall, my week in London was an awesome experience that has me inspired in a lot of different ways. Not only have I got more project ideas bouncing around in my head, but I also have a deeper appreciation for how powerful open source communities can be. Reading the news, the world can seem pretty bleak sometimes (this was driven home by [some of the local events when we were in London](http://www.bbc.com/news/uk-politics-36590824)). Seeing the altruism and work going on even just at one open source organization during this week felt like a bright spot. 

![Mozilla group photo](/assets/images/2016/06/moz-a-0191.jpg)
*All the peeps in London!*
