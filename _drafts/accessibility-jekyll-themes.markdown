---
layout: post
title: Hacking for Accessibility
---

![Hacktoberfest logo]({{site.url}}/assets/images/2019/10/hacktoberfest.jpg)

I went to the Strange Loop conference this year, saw a ton of great talks and now I have more project inspiration than I can shake a stick at. In particular, the talks on accessibility made me excited to do an accessibility themed Hacktoberfest project this year. If you're curious, these talks are below.

### Strange Loop 2019 accessibility talks:
- ["Voice Driven Development: Who needs a keyboard anyway?" by Emily Shea](https://www.youtube.com/watch?v=YKuRkGkf5HU)
- ["Beyond Alt-Text: Trends in Online Accessibility" by Ian Forrest](https://www.youtube.com/watch?v=pNcB7ChyO1E)

These talks made me think about my recent move of my personal blog to Jekyll. I noticed that my it wasn't very accessible for people who use keyboard navigation. I was using a free Jekyll theme, and I found that I couldn't tab to my menu's hamburger button. This meant that anyone using the keyboard to navigate couldn't oprn the menu on my blog. Then I noticed that most free Jekyll themes I checked weren't very accessible. So my plan was to submit PRs to improve accessibility in these open source themes.

It may seem like small potatos to try to fix individual blog themes, but many tech (and other) blogs use Jekyll with free themes. If these blogs are inaccessible, that means their content is inaccessible as well. This limits the information and learning opportunities for people in the web development world and increases inequity. So I'm hoping these small changes can make a significant impact for these readers. Plus as a bonus, it will increase accessibility awareness at the very least for the maintainers of these open source projects.


### Getting Started

Need to enable tab navigation
http://www.weba11y.com/blog/2014/07/07/keyboard-navigation-in-mac-browsers/

Explain tabindex 
https://bitsofco.de/how-and-when-to-use-the-tabindex-attribute/

outline none

### The Pull Requests

https://github.com/minicomp/ed/pull/14

- replace label with button
- add aria label and expanded
- set focus on sidebar when opened

https://github.com/daattali/beautiful-jekyll/pull/559

https://github.com/tomjoht/documentation-theme-jekyll

https://github.com/murraco/jekyll-theme-minimal-resume

