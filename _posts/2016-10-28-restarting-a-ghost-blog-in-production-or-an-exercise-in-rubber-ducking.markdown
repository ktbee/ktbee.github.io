---
layout: post
title: '"Restarting a Ghost blog in production" or "an exercise in rubber ducking"'
date: '2016-10-28 17:06:17'
tags:
- code
---

[SPOILER ALERT] <br>
I figured out that the steps I describe in this post weren't necessary for my Ghost blog after all. So, I recommend holding off using any commands in this post until you read the whole thing. <br>
[/SPOILER ALERT]

I self-host my blog using [Ghost](https://ghost.org/) as my content management system, and I recently troubleshooted ([troubleshot?](http://english.stackexchange.com/questions/3060/which-is-correct-troubleshooted-or-troubleshot)) an issue I thought others might run into: what do you do if `forever` isn't restarting your server like you're expecting it to?

Before I get into the solution I found, I should back up and give a little context on how I've set up my blog. I installed Node.js to act as my server and I'm using the [forever Node module](https://github.com/foreverjs/forever) to ensure my server keeps running even if it runs into some sort of disruption. Forever comes with some handy commands like `forever start [SCRIPT FILE NAME]` and `forever restart [SCRIPT FILE NAME]`, which are useful if you need to restart your Node server. 

### Why am I doing this?
You may be wondering, but why would you want to intentionally shut down your blog's server? The reason for this is that at one point, Ghost required you to restart your server when you added a new theme to your blog. Restarting the server allows the new theme to appear as a theme option in your "General" settings. Without the restart, your blog never recognizes that there is a new folder in its `/content/themes` directory. My guess is that Ghost only checked for themes on start up, and there wasn't another way to trigger this check.

In newer versions of Ghost, you can now upload a new theme through your blog's interface if you have a zipped file for your theme. This is handy because you don't need to restart anything through the command line, but it does have some limitations. If you *do* want to add a new theme's folder via the command line (say because you're cloning a theme from GitHub and don't feel like the extra steps of zipping a folder and using the interface), the new theme still doesn't show up as a theme option without the server restart. Also you may be on an earlier version of Ghost (themes uploads were introduced in 0.10.0), which also means restarting your server for new themes until you get around to upgrading. 

![](/assets/images/2016/10/ghost-busters.jpeg)
<i>I ain't afraid of no ghost</i>

### Some spooky behavior
As I mentioned, I installed the forever and forever-monitor Node modules to make sure by blog runs ... well forever. However, when I try to use `forever restart [SCRIPT]` command, I get the error that the forever process doesn't exist for that script. In fact when I do `forever list`, I get the response that *no* forever processes are running. I haven't fully figured out why that is, but when I manually force my server to stop running, it restarts immediately, so I think some form of forever or forever-monitor is keeping this thing going. It just doesn't respond to my attempts to stop the server using forever as some documentation has said it should.

Since forever commands are a bust, I have been finding the process ID (PID) for the script I want to stop (my server) and using the Linux command `kill` to stop that particular process. To find the PID, I use the following `ps` command to narrow down my results to just Node processes:

```
ps aux | grep node
```

This returns a lot of information, but the first set of digits is what we're interested in because that is the process's PID. Find the process that is listed as `index.js` (or whatever your script's name is) and copy that PID. Then you can use the following command to kill the process:

```
sudo kill [PID]
```

A word of warning though, you may not want to do this on any blog that isn't a personal one like mine. Forcing things to stop in production is probably a bad practice, but for Ghost hobbyists, you might not mind this element of danger. Just make sure you back up your site before killing it 0_0


### Scratch that, I figured it out

![](/assets/images/2016/10/ghost-duck.jpeg)

Well, I feel like we covered a lot of ground and so I hate to break it to you, dear reader, that as I was writing this post, I figured out why I wasn't able to restart my server using forever commands. I wasn't logged into my server as the same user that started the server using forever in the first place, so I wasn't able to see these forever processes as this different user. Once I was logged in as the correct user, I was easily able to restart my server using forever. Boy is my face red!

All that being said, I'm still posting this in the hopes that this info might be helpful to someone struggling to stop any particular process on their server. Plus, you've got to admire the power of [rubber ducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging).

![](/assets/images/2016/10/dancing-skeleton.gif)

<i>Here's hoping you have a spooky Halloween!</i>
