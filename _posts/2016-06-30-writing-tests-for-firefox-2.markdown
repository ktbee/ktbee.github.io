---
layout: post
title: Writing tests for Firefox
date: '2016-06-30 19:24:12'
tags:
- code
- mozilla
---

Writing unit tests is one part of the software engineering world that I didn't have much experience in before my internship with Mozilla. I knew it was important in theory, but when working on solo projects there always seemed to be another more pressing aspect to work on. Also when I broke my code, I could see it was broken and fix it. Writing a test would just tell me what I already knew, right?

![Students taking a test](/assets/images/2016/06/Shimer_comping_66_Recondite.jpg)
*After you graduate, you still have tests. In fact, now you write them.*


### Why testing is important
A big reason for writing tests is to make your code maintainable in the long term. For my personal projects this benefit is a little less obvious because I don't typically collaborate with others or have projects that have spanned years and years (yet), but for Firefox this maintainability is crucial. It's a large codebase with a lot of contributors. I found having tests made my contributions a lot easier because I could tell immediately if my carefully crafted code was breaking another part of the software. 

As I'm starting to expand features (instead of just fix them), I've also found that writing tests has been helpful for my thinking process. I've found that asking myself some basic test questions has uncovered some bugs even before I wrote the test. In particular I've found the questions below are good to fully articulate to myself: 

- What am I expecting to happen? 
- What will the inputs be?

And then writing tests around these questions helps me catch even more mistakes as I iterate on your original design. 

If you'd like more examples for why you should test, you can check out the stack overflow post linked below. It's an interesting discussion on why developers test and how they go about it. 

[http://stackoverflow.com/questions/3258733/new-to-unit-testing-how-to-write-great-tests](http://stackoverflow.com/questions/3258733/new-to-unit-testing-how-to-write-great-tests)

### When should I write a test?
When first starting out, I wasn't sure what pieces of code were significant enough to have a test written for them. I asked [my mentor](https://msujaws.wordpress.com/) generally when I should add to a test or even create new ones, and he had some helpful advice:

> My rule of thumb for writing a test is to see if it matches the following criteria:

>1. Is this a regression? If this was working before and is now broken, it's likely to get broken again.

>2. Is this verifiable through some boolean check? This may take some more experience and creativity, as some things may sound hard to test until a more creative or subversive approach is thought of which might be easy to test.

>3. Is the effort of writing a test less than 3x the effort of writing the production code? Writing tests often takes more time than the actual fix, but because it ensures that the changes will live on longer it is worth it. If it is going to take more than 3x the work, then it's probably not worth it.

These have been good guidelines for knowing when a code reviewer will want me to add a test to a patch I'm writing. 

### Asynchronous testing

![Time is hard](/assets/images/2016/06/The_Persistence_of_Memory.jpg)
*Time is hard*

When I started writing tests for Firefox, one stumbling block I ran into was allowing enough time for a process to finish running (like a function triggered by a mouse click) before the test moved onto the next step of checking its results. To make sure everything in your test finishes processing before you check it, you can write your test to be asynchronous. 

Going back to the click event example, you can write an asynchronous test for the function that is triggered by a click by using [generator functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*). Generators are JavaScript functions that allow you to pause them until you get a value from an expression.  After the click, you can make the generator function (and the entire test) wait until the action that was triggered finishes by using `yield` on your expression. The code snippet below is in a test I'm working on for a button that can indicate your browser's zoom level and also reset to the default zoom level when clicked. It is executed as part of a generator function.  

```
let zoomResetPromise = promiseObserverNotification("browser-fullZoom:zoomReset");
zoomResetButton.click();
yield zoomResetPromise;
let pageZoomLevel = Math.floor(ZoomManager.zoom * 100);
let expectedZoomLevel = 100;
is(pageZoomLevel, expectedZoomLevel, "Clicking zoom button successfully resets browser zoom to 100%");
```
The expression here is `zoomResetPromise`. In the test I have a click event happen on the button, but then I use `yield` to pause the generator function until we get the response we need from `zoomResetPromise` after the button click. Once we have the all clear that the zoom reset process has finished, we can use the next function to check whether our expected outcome is the same as what we get from the zoom reset.

If you're interested in learning more about generator functions, the articles below helped me wrap my mind around them.

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield)

[https://davidwalsh.name/es6-generators](https://davidwalsh.name/es6-generators)

### Testing tools available for firefox
There is some great documentation and tools out there for writing tests for Firefox. Take a look at the links below for some guidelines on how to write tests and frameworks available for creating expressions you want to yield.

Mochitest
https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Mochitest

Guide to browser tests
[https://developer.mozilla.org/en-US/docs/Mozilla/Browser_chrome_tests](https://developer.mozilla.org/en-US/docs/Mozilla/Browser_chrome_tests)

Built-in testing library. I would read through the comments in this file before starting your test to make sure you don't re-invent the wheel.
https://dxr.mozilla.org/mozilla-beta/source/testing/mochitest/BrowserTestUtils/BrowserTestUtils.jsm
