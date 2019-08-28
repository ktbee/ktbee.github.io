---
layout: post
title: Where to handle redirects when using Node and Vue
date: '2018-06-10 19:17:21'
---

Recently I've been working on a web app that uses Node as its server and Vue.js in the front, and I found that I had a number of pages that needed their URL changed from something like `example.com/a1` to `example.com/a2`. My first instinct was to take the easiest route (ha) and just add a bunch of redirects to the Vue router to have something like this:

```
const appRouter = new Router({
  mode: 'history',
  routes: [
    { path: '/a', redirect: '/a2' },
    { path: '/b', redirect: '/b2' },
    { path: '/c', redirect: '/c2' },
  ]
});
```

Though I had planned to have path and redirect mappings live in another file to keep my code looking neater, this still struck me as pretty hacky/inelegant. What if someone has JavaScript turned off? This would break the page redirect. Also is Vue really meant to handle the large number of granular redirects that comes from migrating a lot of pages to another URL pattern?


### You got served
I got some good advice from the BostonJS Slack and StackOverflow (thank you everyone!), and realized that these redirects should live on the server. If the server handles the redirects before page loads, we can return a 301 Move Permanently status code, which is the best practice for moving the same resource to a new location. Also these redirects aren't part of the app's logic, so we should keep things tidy by only adding routes that are necessary for views in the app.

<div class="img-center">
<iframe src="https://giphy.com/embed/ayxtic9I9AUko" width="480" height="260" frameBorder="0"></iframe>
<p>Let's never forget about this goofy guy</p>
</div>


For the curious, this is how I set up the redirects on my Node server:

#### server.js
```
const redirectMappings = require('./redirects.js');

app.use( function redirects(req, res, next) {
  const urlPath = req.path.split('/');
  const legacyId = urlPath[ 1 ];

  if ( redirectMappings[ legacyId ] ) {
    return res.redirect(301, '/' + redirectMappings[ legacyId ].id );
  }

  return next();
});
```

#### redirects.js
```
module.exports = {
  'a1': {
    name: 'Page A',
    id: 'a2'
    },
  'b1': {
    name: 'Page B',
    id: 'b2'
  },
  'c3': {
    name: 'Page C',
    id: 'c3'
  }
}
```

Strictly speaking, I didn't need to have a `name` associated with each redirect entry, but it was helpful for me as a human when checking to see if I had added a redirect for a page yet or not. 

### Why even have client side routing?

All of this got me thinking, what is the use case for client-side routing? It's handy in apps for improving user experience for a few reasons:

- You don't need a full refresh of the page when directing the user to a new location, which can make for a less disorienting transition.
- It's more efficient and feels faster to only change the parts of the DOM you need to update, rather than downloading a whole new web page and its associated JS and CSS.
- You can add stylish loading transitions between page changes.

Thinking existentially about client-side routing, it seems like going through the trouble to make the page update feel like a classic page redirect (like updating the URL in the browser, styling the DOM content to look like another page even if there is no hard refresh) , is just a convenience for the user to signal that they should think of themselves as visiting a new place now. I guess the inverse of this approach is those "load more posts" links you see on blogs that trigger adding more data (posts) to the page on click. Here the user isn't meant to feel like they're in another place, so they're not routed to another URL and the page doesn't change much. However, it's effectively the same thing when you fetch more data from the server and update the page asynchronously. 

### Client-side routing beyond the app

Typically you see client-side routing used for single page apps, but we also used it for recirculation on [the new Harvard Gazette website.](https://www.upstatement.com/work/harvard-gazette/) I used PJAX ([the browser's `pushState` API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) + AJAX) to load the next story displayed at the bottom of an article page and change the URL in the browser when the visitor clicks on it. For example, take a look at this story and click the article that's promoted in the navbar or at the bottom of the page under "Up Next":

https://news.harvard.edu/gazette/story/2018/05/newburyport-artists-spirit-books-on-display-at-arnold-arboretum/

Our designer spent some time adding a nice slide transition, and I really like the effect. The next story starts loading as you scroll down the page, so it is ready to display as soon as the user clicks the article link. The fun transitions and snappy loading encourage user engagement and clicking through the next story. A big improvement over a full page refresh and waiting for data from the server!