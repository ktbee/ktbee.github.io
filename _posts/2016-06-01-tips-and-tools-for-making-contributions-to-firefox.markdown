---
layout: post
title: Tips and tools for editing Firefox source code
date: '2016-06-01 21:22:00'
tags:
- code
- mozilla
---

### Getting started is half the battle
To make contributions to the Firefox desktop app, you need to clone a copy of Mozilla's code base and be able to run it on your local computer. I'm sure this is true of Mozilla's other projects as well, but since I'm working with the Firefox browser, I can speak best to that one. Once you have a version of Firefox's code running locally, you can start to tinker with the source files, compile them, and then take a look at how great your changes are (probably pretty great! ... on about the millionth try). 

### Tips for setting up your local environment
While the idea is simple enough, I found that building my local copy of Firefox can come with some challenges. [There is a lot of good documentation out there on how exactly to go about it](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions), so I won't re-hash it. However, I will offer some advice from the issues I've run into most often. I've gone through this process once on OS X and a few times on Windows. In this small sample size I found Windows was the most challenging. These tips mostly come from my Windows efforts: 

- Before you start, add an exception to your anti-virus software for your Mozilla source code folder. It is very likely it will think what you're downloading and running is a virus and prevent your processes from completing, but it may not let you know this. This has been the culprit for me a couple of times.

- Run your terminal or command line as an administrator when you go to start your download and build process. This will avoid permissions issues. 

- MozillaBuild has a really neat UNIX-like tool that I didn't understand at first. [You can read more about it here](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Windows_Prerequisites#MozillaBuild), but it basically gives you a UNIXy command prompt when you run it. Once installed, you simply cd into your mozilla-build folder in your command line and type the name of the script (ie "start-shell-msvc2015.bat"). That's it, just typing the name of the script will start it and allow you to use Mercurial. You won't need to install Mercurial separately. 

- When you first build your Mozilla source code, it will likely take a very long time. However for my first Windows build, I found it took over 24 hours. Even though I was using an old laptop, this seemed extreme. My mentor mentioned that my power settings were likely the issue, and indeed they were. Make sure your computer is charging and its power settings prevent it from going to sleep when plugged in (even if temporarily). My computer had been going to sleep and not building except for every so often when I would check it.  

### Tool time!
![tool time!](/content/images/2016/06/tim-allen.gif)
<br>

While troubleshooting, these have been my most useful resources so far:

#### [Browser toolbox](https://developer.mozilla.org/en-US/docs/Tools/Browser_Toolbox)
If you're familiar with web development, you've likely used "inspect element" dev tools to take a look at the code on a page and seen a bunch of useful information like CSS rules and console errors. Well, I hope you're sitting down while you're reading this, because it turns out Mozilla has the same thing ... for the browser itself. It's called the [Browser toolbox](https://developer.mozilla.org/en-US/docs/Tools/Browser_Toolbox), and it allows your to inspect elements of the browser like you traditionally would a web page. Perhaps my readers won't be as fazed or excited about this capability, but I found inspecting the browser itself to be a little bit like pulling back a curtain and finding the emperor of Oz was a kindly little dude. Firefox is a piece of software that I have been using since well before I understood any code, so realizing it was something I could take apart and understand was empowering. Even if you don't plan to contribute to Firefox, I recommend taking a peek.


#### Search MDN documentation, Bugzilla
When learning to make web apps, I would often find myself looking at Mozilla Developer Network articles to learn how a particular part of the web should work. It turns out they have a lot of good articles on how the Firefox code base works as well. Here's a couple:

[Mozilla Source Code Directory Structure](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Source_Code/Directory_structure)

[XUL user interfaces, Mozilla's specialized language for creating user interfaces](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started/XUL_user_interfaces)

I also found searching [Bugzilla (Mozilla's bug tracking system)](https://bugzilla.mozilla.org/) to be good as well for seeing how developers have approached similar problems in the past. The public bugs are indexed by Google, so you may find Bugzilla bugs there as well. Keep in mind though that it won't show any bugs that haven't been indexed by Google yet. 

#### irc/talking to your mentor if you have one
I've found that sometimes it's easy to get stuck in a rabbit hole and end up spinning your wheels on a problem. You can save yourself some time and angst by reaching out with your question. I try to exhaust my searching and troubleshooting options before I ask my question to make things as efficient as possible. When I do finally reach out for feedback though, I've found it immensely helpful in getting unblocked.