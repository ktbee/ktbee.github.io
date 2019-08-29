---
layout: post
title: Exploring the dis/u/topia in our media landscape
date: '2018-01-29 04:27:59'
tags:
- code
---

I've been working on a fun JavaScript art project lately that I've been calling [Youtopia](http://youtopia.world). It's my interpretation of how someone living in the 80s would picture our mediascape if someone described the internet to them. How would a 1980’s dweller visualize the ability to watch as much media as you want, whenever you want? 

Ideally you watch Youtopia from a tube TV, which is surprisingly easy to do if you have a digital to analog converter. If not, It's still pretty good on your computer. However when I watched it on a tube TV, it was amazing how much I was transported back to a time before the internet. Sometimes I would find myself wanting to pause a video, then think I couldn't because it was a TV show, then realize I could because it was just the TV on the internet!


![](/assets/images/2017/12/buns-of-steel-1.gif)
<i>Results for the search term "Buns of Steel"</i>

<h3>An art project</h3>

The premise of Youtopia is simple, but it lends to some intriguing juxtapositions and more complex meaning from the video combinations that turn up. When you arrive at [http://youtopia.world/](http://youtopia.world/), you see four YouTube videos that are related by a common search term. I made it it so that the app searches for one random word, and returns the first four results. You can see the search term in the developer's console.

    
![](/assets/images/2018/01/could.gif)
<i>For the term "could" we get two music videos, an ad, and a live stream about exorcising demons from the White House</i>

One thing I love about this project is the surprising layers of meaning that are created from seemingly innocuous search words or phrases. I've made it possible to search for terms if you add them to the end of the youtopia.world URL, for example "[https://youtopia.world/your search term here](http://youtopia.world/your%20search%20term%20here)". 


![](/assets/images/2018/01/Screen-Shot-2018-01-28-at-9.42.47-PM.png)
<i>Sorting algorithms and Arabic chants for the term "sort"</i>


Below are some of my favorites:

* [Clint Eastwood](http://youtopia.world/clint%20eastwood)
* [Russian aerobics](http://youtopia.world/russian%20aerobics)
* [ASMR](http://youtopia.world/asmr)
* [Blue velvet](http://youtopia.world/blue%20velvet) (this one is a slow boil)
* [Video art 1970](http://youtopia.world/video%20art%201970)

I love the zaniness of these results, but my favorite thing about this project is how much it gets me out of my own media bubble. I never would have been exposed to most of this stuff in my own media habits, especially some of more obscure live streams I've seen when I hit (what I think is) the endpoint to see the most recent live stream on YouTube, which is what I use for [http://youtopia.world/live](http://youtopia.world/live). Some gems I've found on the live streams:

* Spanish-speaking trucker filming himself and the road, Blair Witch Project style
* Homemade political talk shows, usually bashing Hillary Clinton. 
* Another spanish-speaker showing off the good deal she got on dozens of bottles of shampoo
* Endless video games, MMA matches, and sports games

One thing I didn't realize before this project was how conservative YouTube tends to be as a platform. The results are the top few from a random search, but conservative topics (whether politically or socially) repeatedly pop-up every so often. 

I've gotten a qualitative sense of how conservative YouTube is, but I heard some quantitative evidence for this at a recent event at [Cortico, a research non-profit](https://www.cortico.ai) that works with journalists to analyze social media networks. They have [some really cool work on the bubbles and silos in Twitter](http://www.electome.org/), and they also gave a presentation on other networks they're digging into. They mentioned that Youtube skews fairly red, despite it being seen as a neutral platform. I was excited that their findings were consistent with my experience. I'll be interested to see what other stories and collaborations they come out in the future on other networks. 

<h3>Interesting code project</h3>

This project has been a fun way to explore the YouTube API. [The heart of Youtopia is a wrapper for different search terms and the YouTube API.](https://github.com/ktbee/youtopia/blob/master/public/controller.js) I also made a really small, simple code splitting system by dividing the JavaScript each endpoint needs on the front end in its own file, and then I pass that file name to the client as part of the endpoint's response. For example for the home page, this is the wrapper's endpoint:

```
exports.index = function( req, res ) {
  res.render('four', { script: 'index' });
}
```

And this is the script tag in the view's template:

```
<script src="assets/js/{{ script }}.js"></script>
```

I enjoyed thinking about the kind of optimizations I'd get with using Webpack, but it would have been overkill for a project this small. I liked thinking about how I could keep it simple and avoid a lot of tools and libraries bloat.

<h3>Next steps</h3>

Youtopia has turned out to be way more interesting and fun to use than I expected, but there are a few improvements I'd like to make. Namely, I'd love to make it so that users can bookmark certain groupings of videos, and not just the term. I'm thinking the videos' IDs would be added to the URL, so that they can easily be shared ... perhaps on social media :)

I'd also like to see this project have a physical component. I could see it playing on a number of TV sets at once, perhaps also creating some other visualizations to play along with it. 
