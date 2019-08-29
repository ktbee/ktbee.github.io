---
layout: post
title: Simple apps for complex emotions
date: '2018-03-18 20:01:57'
tags:
- code
---

Recently I made a couple apps with [my friend Emily Theis](http://emilythe.is/) based on podcasts by [WNYC's Death, Sex and Money](https://www.wnycstudios.org/shows/deathsexmoney). This first one is [breakupsurvival.guide](http://breakupsurvival.guide/), which is [based on listener submitted breakup survival suggestions.](https://www.wnycstudios.org/story/cut-loose-breakups-death-sex-money/) Initially all of these suggestions were put together by Death, Sex and Money in a Google Sheet, but Emily noticed there was always other people looking at [this Google Sheet](https://docs.google.com/spreadsheets/d/1M42gLZLANOxXxl9-N2MEYcw0FW9ng2fSRhaLtrMykw0/edit#gid=1875877531) along with her. 

<div class="img-center" markdown="1">
![Google Sheet screenshot]({{site.url}}/assets/images/2018/03/Screen-Shot-2018-03-18-at-3.43.06-PM.png)
<i>Are you feeling the same things I am, Anonymous Camel? </i>
</div>

These anonymous comrades made Emily want to create a mobile-friendly website version of the Google Sheet. She created the frontend of the site, and I hooked up a copy of the suggestions Google Sheet as a very simple database/Content Management System. 

I was interested in this project for a couple of reasons:

1. I wanted to challenge myself to keep the code as simple as possible, especially after working with more elaborate build systems and frontend frameworks for a while. 
2. I had heard about people using Google Sheets as a CMS, but I hadn't explored that technique and was curious about what tools were available for it.
3. The subject matter was something that a lot of people care deeply about.
4. I could help my friend improve her coding skills.

The library I ended up using for this project is called [Tabletop](https://github.com/jsoma/tabletop), and setting it up was really simple. [I didn't even really need to do much beyond the example code:](https://github.com/emilytheis/breakup-survival/blob/master/js/app.js#L18-#L30)


```
 function getCardData() {
    Tabletop.init( { key: 'https://docs.google.com/spreadsheets/d/1ZqCUv_Ps0lHS0_I8Onk_xcdP9ThUS2ALtmxre5o7h5Q/pub?output=csv',

    callback: function(data, tabletop) {
      tabletopData = data;
      randomizeData(tabletopData);

      if($('.beating-hearts-baby').length) {
        $('body').removeClass('beating-hearts-baby');
      }
    },
    simpleSheet: true } );
}
```

And then I used the same column names as my HTML element IDs and injected the column content with the following code whenever the "Next Suggestion" button was clicked:

```
var suggestionData = data[dbRow];
var elements = ['type', 'name', 'suggestion', 'comment'];

// Grab the content and put 'er in
elements.forEach( function (el, index, elements) {
  elements[index] = document.getElementById(el);
  elements[index].textContent = suggestionData[el];
});
```

The one thing that was tricky was that at one point we accidentally had a second blank sheet as part of the Google Sheet we were pulling from, which caused an error and prevented information from being sent back to the app. This was because we told Tabletop to expect just one sheet (`simpleSheet: true`). Once I enabled [debug mode](https://github.com/jsoma/tabletop#debug) however, the error message and the cause of the issue was super clear. I just didn't realize this handy mode existed for a while and so I wasn't seeing the error message.

[The code for this app](https://github.com/emilytheis/breakup-survival/blob/master/js/app.js) could be improved in a few ways (namely by caching the Sheet data,  caching the HTML element reference so you don't have to get the elements by ID on each click, and adding a loading animation), but for a quick and fun project it worked well enough. We made an app-y website that a ton of people visited, that got [some German press](https://www.jetzt.de/liebe-und-beziehung/emily-theis-hat-den-breakup-survival-guide-programmiert) and that Death, Sex and Money were excited about! Plus another bonus: turning on Google translate for that German article introduced me to such gems as "grief hole."

<div class="img-center" markdown="1">
![bsg-demo](/assets/images/2018/03/bsg-demo.gif)
</div>


<div style="display: flex; justify-content: space-around; padding: 1em">

<blockquote class="twitter-tweet" data-cards="hidden" data-lang="en"><p lang="en" dir="ltr" style="margin: 0 auto;">Our listener Emily <a href="https://twitter.com/the_is?ref_src=twsrc%5Etfw">@the_is</a> made a very cool thing from YOUR breakup survival tips: <a href="https://t.co/dhEuX9z0Q8">https://t.co/dhEuX9z0Q8</a></p>&mdash; Death, Sex &amp; Money (@deathsexmoney) <a href="https://twitter.com/deathsexmoney/status/893918291613413377?ref_src=twsrc%5Etfw">August 5, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

</div>

In fact, DSM ended up liking the project enough to ask us to do another one for their [Opportunity Costs series](https://www.wnycstudios.org/story/opportunity-costs-death-sex-money-podcast) about the role class identity plays in our lives. In a similar vein as the Breakup Survival Guide, it shows randomized listeners' responses to the question, what makes you proud or ashamed of your class? You can find this app at [http://dsmclass.community/](http://dsmclass.community/)

<div class="img-center" markdown="1">
![class-demo-1](/assets/images/2018/03/class-demo-1.gif)
</div>

[For this second round](https://github.com/emilytheis/dsm-class/blob/master/js/app.js), I used `localStorage` in the browser to made caching improvements for both the Sheet data and also for the HTML elements:

```
cacheData: function( data ) {
    var scope = this;
    var dataToCache = JSON.stringify( data );
    var today = new Date();
    var cacheName = 'DSMclass' + today.getMonth() + today.getDate();
    var cachedData = localStorage.getItem( cacheName );

    // Only clear and create new cache if today's data hasn't been cached yet
    if ( !cachedData ) {
      localStorage.clear();
      localStorage.setItem( cacheName,  dataToCache);
    }

    return;
}
```

And I also created [a service worker to make this website a little more of a Progressive Web App](https://github.com/emilytheis/dsm-class/blob/master/service-worker.js). This allows for the HTML and CSS of the page to be cached, which would allow this site to be available even if the user doesn't have internet access. One downside is that it makes site development a little hard because it caches styling, which would mean it would be a roadblock to Emily's frontend work since it wouldn't show any updates she made. I have the service worker's init line commented out at the moment, but now that this site is launched and stable, we'll uncomment it. 

I'm pleased to say that we got a lot of positive feedback and visitors to this Class app as well. People tend to come and stay on these apps, even for up to a minute at a time, which seems like an eternity on the internet. It makes me really excited that a simple premise and website can be so powerful for users. It feels a bit like an art project in the style of [Post Secret](https://postsecret.com/), which is also vehicle for people to anonymously share their innermost thoughts on taboo subjects.

<div class="img-center" markdown="1">
![dsm-class-1](/assets/images/2018/03/dsm-class-1.png)
</div>

Aside from the community response, I found these projects had a few other upsides as well. The tech stack is very simple thanks to the Tabletop library for Google Sheets, which made it easy to get started quickly and simply. I could see this being very helpful for anyone that wanted to get up a quick app prototype. Its simplicity also seemed like a bonus for Emily in terms of learning about the backend and for launching the site.  I highly recommend using Tabletop and Google Sheets if you're a beginner looking to create an app without needing to deal with a complicated tool set. 

Next up I'm working with a friend on an app to both randomly suggest *Sex and the City* episodes to watch and to track which episodes have been suggested. The random suggestion part is very similar to these previous two apps, but for sending information back to the Google Sheet (ie recording with episodes have been suggested), I'll need to go beyond Tabletop since it currently provides read-only data and doesn't allow sending information back to the sheet. 

