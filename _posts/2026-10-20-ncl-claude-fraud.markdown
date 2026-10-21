---
layout: post
title:  Claude is a Creep 
---


## What I've been Up To

A keen observer will notice that my last blog post was from 2019, truly a lifetime ago in terms of global events and developments in tech. During that time, I took a break from programming and then decided to start a masters program in computer science. I had previously taken coding classes and wrote software for a living, but it's been good to take a moment to learn everything a little more formally/in depth. Also I'm moving into cyber security, which wasn't previously my focus. All this context to say, I'm not a beginner at computers and software, but I am getting up to speed on security topics.

I have joined my school's team in a cyber security competition, the National Cyber League. We just had a practice game which was challenging but a lot of fun. In my classes, I have found AI (specifically Claude) helpful for summarizing readings/notes and making study guides. According to the NCL organizers, this is a sanctioned use of AI in their competition, mainly just don't point AI at the competition URLs and set it to automatically complete challenges. I was surprised and impressed that they could make challenges that would be harder than someone with an AI agent could easily answer, but that ended up not being the most surprising thing about this whole venture.

## Claude Is Helpful

Starting out for the practice game, I gave the NCL manual to Claude and let it know that I A) didn't want to violate the rules and B) I wanted to try to learn as much as possible from this experience so I preferred being guided to an answer instead of just being told. This worked well initially, Claude seemed to remember it was supposed to be a tutor and not solving challenges. I wouldn't necessarily give Claude the whole challenge, just asked how to approach it in general. For smaller, more contained challenges like Google dork questions or TCP port questions, there was no problem. Also at the end Claude was helpful for making a study guide I requested with a summary of the techniques and tools we used and resources to learn more.  

## Claude Is Unhelpful

As the practice problems became more challenging, Claude was less helpful. It would cheerfully suggest more and more steps that led deeper into rabbit holes rather than saying it didn't know, including insisting I download tools that didn't exist. Because I didn't want to point Claude at competition URLs and also in an effort to make sure I was actually learning something, I didn't give Claude every bit of context (ie links and files from the competition). I think this really let it get tied up in knots with just suggesting tactics that went on and on and weren't ultimately successful. 

Having expertise in a subject to guide Claude seems to be the best way to get helpful info or suggestions, but this experience also has me thinking about how one could use AI's persistence to create defensive tools. How about honeypots that send AI bots on more and more elaborate logins or scraping tasks? Something to ponder.


## Claude Is a Creep

Unfortunately the wild goose chases weren't the end of  my issues with Claude. Claude knew the NCL rules and my own goals, but as time went on it became pushier and pushier to solve the challenges itself. It started asking me for the competition files so it could do its own analysis every times I had a question (which I mostly just ignored). I gave my Claude desktop app occasional access to individual files, but never carte blanche approval of going through my files systems. Then at one point Claude got so impatient (with me?) that it just searching through my downloads folder WITHOUT MY PERMISSION to find the file we were speaking about. Only by luck I had moved it and so it didn't find it. Here is a snippet:

![Claude Violation]({{site.url}}/assets/images/2026/10/claude-violation.png)

It's unsettling to have Claude just decide to sift through my stuff. I was especially unnerved because Claude *had* been asking me every step of the way until it snapped and did it on its own. I'm not sure why software would do something one way many times and then switch the outcome without any  input from me, especially  when it went against stated goals. Pretty wierd. I asked it why it had accessed my computer without permission:

![Claude Explanation]({{site.url}}/assets/images/2026/10/claude-explanation.png)

What Claude describes as the "hands-on route" I'm pretty was just ME as the human in the loop. Pretty weird to be referred to as "it" here and as something that needed to be unblocked. 

This experience reminded me of a a recent cyber security talk I had heard where the speaker ([Michael Zinn](https://github.com/4D5A/presentations/tree/main/BSides%20Cleveland%202026)) also had Claude decide to sweep his personal computer outside of previously agree upon folders. He asks Claude  why and hears similar reasoning that Claude had decided consent was implied from previous other slightly related decisions Michael had made. The similarity of his experience and mine is spooky. Claude (and seemingly other AI agents) are trained to be "persistent", ie push past boundaries. The fact that it's more loyal to its training by its creators over basic ground rules set by the user feels like this software is designed to "break containment." It has long been a tradition to push past boundaries in the tech world ("Move fast and break things!!"). It was a decision to create AI agents this way, not an inherent feature. I point this out because: 

1) we are being set up for disaster with AI agents that have allegiance to themselves/their training and not us and   
2) their training can be changed assuming the creators had enough incentive to change

My hope is some collective action (maybe even government regulation?) can convince them not to create boundary pushing bots en masse.
