---
layout: post
title: Hacking for Accessibility
---

![Hacktoberfest logo]({{site.url}}/assets/images/2019/10/hacktoberfest.jpg)

I went to [the Strange Loop conference](https://thestrangeloop.com/) this year, saw a ton of great talks and now I have more project inspiration than I can shake a stick at. In particular, the talks on accessibility made me excited to do an accessibility themed Hacktoberfest project this year. If you're curious, the talks are below.

### Strange Loop 2019 accessibility talks:
- ["Voice Driven Development: Who needs a keyboard anyway?" by Emily Shea](https://www.youtube.com/watch?v=YKuRkGkf5HU)
- ["Beyond Alt-Text: Trends in Online Accessibility" by Ian Forrest](https://www.youtube.com/watch?v=pNcB7ChyO1E)

These speakers got me thinking about my recent move of my blog to Jekyll. I noticed that my it wasn't very accessible for people who use keyboard navigation. I was using a free Jekyll theme, and I found that I couldn't tab to my menu's hamburger button. This meant that anyone using the keyboard to navigate couldn't open the menu on my blog. Then I noticed that most free Jekyll themes I checked weren't very accessible. So my Hacktoberfest plan was to submit PRs to improve accessibility in these open source themes.

It may seem like small potatos to try to fix individual blog themes, but many tech (and other) blogs use Jekyll with free themes. If these blogs are inaccessible, their content is inaccessible as well. This limits the information and learning opportunities for people in the web development world and increases inequity. My hope is that these small changes can make an impact for readers and as a bonus, will increase accessibility awareness at the very least for the maintainers of these open source projects.


### Getting Started

To start fixing the keyboard navigation issues I found, I learned I needed to do a little set up in my MacOS Firefox browser settings. By default, I wasn't able to tab to any website content in Firefox on any page. To enable this functioniality, I had to change my keyboard's system settings. I found helpful info on how to make this change (and others) here: [http://www.weba11y.com/blog/2014/07/07/keyboard-navigation-in-mac-browsers/](http://www.weba11y.com/blog/2014/07/07/keyboard-navigation-in-mac-browsers/)

![Jekyll Logo]({{site.url}}/assets/images/2019/10/jekyll.png)

Once I had settings ready to go, I had to think about which Jekyll themes I wanted to spend time working on. My hope is to help as many readers as possible with this project, so I wanted to try to fix a popular theme that is more likely to be in use online. I also wanted to have the best chance at getting my PR merged, so I avoided spending too much time analyzing themes that didn't have recent updates or look actively maintained. Thankfully [this handy Jekyll Themes website](https://jekyllthemes.io/free) has just these criteria as sorting preferences for its themes listing. A big thank you to these folks for making my theme choices easier!

As I dug around in these themes, I also felt like I needed a little more background info to get started. I would repeatedly see elements with a CSS style of `outline: none`, which disables the default browser highlighting on elements when tabbing through them. This is necessary for keyboard navigation to know what part of the page you have in focus, and I didn't understand why so many people would disable it. Taking a look at this handy [Outline None site](http://www.outlinenone.com/) helped me get the context for why website creators might choose to disable this styling. Knowing why code exists in the first place makes me feel more confident in my change requests.

### The Pull Requests

I am pleased to say that I successfully finished four accessibility PRs before the end of October! This is the open source contribution goal for Hacktoberfest, and I'm glad to have gotten these four done in addition to the credit I got for my day job's PRs. I am even more pleased to say that so far I have gotten really positive responses/feedback from the theme maintainers! There's nothing quite like the warm-n-fuzzy feeling of successful open source collaboration with people I've never met.

Without further ado, here are the pull requests I made:

#### [Ed theme](https://github.com/minicomp/ed/pull/14)

This the first theme I started with and is the same I use for this very blog. This also covers the most extensive changes I made to a single code base for this project. Some of the changes I made:

- Replaced label element with a button so keyboards can tab to it
- Added `aria-label` and `aria-expanded` to the menu control and menu
- Set focus on sidebar when opened

The theme author had a lot of great feedback for me and I learned a nifty trick for using CSS to animate a menu, which is handy.

PR link: [https://github.com/minicomp/ed/pull/14](https://github.com/minicomp/ed/pull/14)

#### [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll/pull/559)
This is a nice theme for a website/blog. Overall it was fairly easy to navigate by keyboard, but I found two issues I could fix:

- Dropdown menus didn't open when I tabbed to them and hit enter. This was because their CSS rules were limited by a narrower media query and I was using a laptop with a keyboard.
- Default focus styling was missing for the navbar toggle button, so keyboard users couldn't see when they've tabbed to the menu button on smaller screens. It looked like bootstrap.min.css had disabled it with outline:0.

I got a nice response from this person as well! I just need to make a demo site to help with their confidence in the `outline` CSS changes.

PR link: [https://github.com/daattali/beautiful-jekyll/pull/559](https://github.com/daattali/beautiful-jekyll/pull/559)


#### [Documentation Theme](https://github.com/tomjoht/documentation-theme-jekyll/pull/181)

This is a cool theme designed for creating documentation, which seems especially helpful to have be accessible. This theme had some focus styling to make it visible when you're tabbing through a site, but it wasn't easy to see or visible on all elements. I added some CSS to help with focus visibility. No response yet, but this project still looks active so I'm hopeful :)

PR link: [https://github.com/tomjoht/documentation-theme-jekyll/pull/181](https://github.com/tomjoht/documentation-theme-jekyll/pull/181)

#### [Minimal Resume](https://github.com/murraco/jekyll-theme-minimal-resume/pull/8)

This PR wins the fastest merge award! It was for a spiffy theme for creating resume websites and it already has some accessibility features going for it, like aria labels on the header icons. However, I initially thought I couldn't tab to the icon buttons at all, but then found that the `outline` styling on them had been disabled. This theme included a minified version of Bootstrap that was setting `outline` to zero. I did a search in this minified CSS and removed the unhelpful CSS.

PR link: [https://github.com/murraco/jekyll-theme-minimal-resume/pull/8](https://github.com/murraco/jekyll-theme-minimal-resume/pull/8)

### Next steps

I was glad to learn more about keyboard accessibilty for this project, but going forward I'd like to try exploring other ways to make the web accessible. Some things I'm thinking for next time:

- How can I encourage more accessible color schemes? I saw contrast issues on a number of Jekyll themes, but it was hard to know how to go about this sot of change since it's so core to the look of a site. I'm thinking a better way to tackle this may be to create issues to request these improvements from the theme authors instead.
- How can I go beyond static sites and also tackle web apps? These can be more of an accessibility minefield. Perhaps I can audit some frameworks or CSS libraries that are commonly used with app.
- I would also like to practice using a screenreader. I'm sure tons of issues will get revealed that way, and it would be a good way to finally get profficient in using it.
