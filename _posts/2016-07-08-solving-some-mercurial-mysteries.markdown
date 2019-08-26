---
layout: post
title: Solving some Mercurial mysteries
date: '2016-07-08 17:04:48'
tags:
- mozilla
- code
- version-control
---

[**edit 7/12/16:** This post goes through some pretty specific issues I had, but if you're looking for more general information on how Mercurial works and what your workflow should be like, you can check out a fellow Outreachy intern Anjana Vakil's post: [http://vakila.github.io/blog/warming-up-to-mercurial/](http://vakila.github.io/blog/warming-up-to-mercurial/). I found it very helpful!]

<div style="float: right; width: 300px;">

![Mercurial logo](/content/images/2016/07/mercurial-logo.png)
</div>

Mozilla uses a version control system called Mercurial, which was new to me as a Git user. While overall I like Mercurial, I've had some growing pains in terms of both getting used to a new system and getting used to collaborating with other developers in general. I solved a couple issues this week, so I thought I'd share. 

### Mystery 1: How to keep my local repo up-to-date
In one patch I've submitted, I ran into the issue where my local repo ("repository" -- a copy of the Mozilla code base on my computer) had been out of date when I pushed my changes, and so my updates didn't merge well with the mozilla-central repo. Going forward I wanted to make sure my repo was up to date before submitting patches, but how? [Following some documentation](http://mozilla-version-control-tools.readthedocs.io/en/latest/hgmozilla/firefoxworkflow.html#feature-development), I found that I could pull in updates using `hg pull central` and `hg up central`, but this would create a new head (or [DAG branch](http://mozilla-version-control-tools.readthedocs.io/en/latest/hgmozilla/dag.html#the-dag-and-mercurial)) in my local repository and the head I was working on didn't have the updates. It sort of looked like this:

```
mozilla-central
o  
| o my-sad-out-of-date-patch :(
|/
|
|
```

Generally speaking it makes sense to do something like `hg merge` to bring my patch's branch up-to-date, but MozReview (Mozilla's code review system based off of Review Board) doesn't allow for merge commits. This is the reasoning:

>Reviewing merge commits is wonky because the diff of a merge commit is ambiguous and can be deceiving. Furthermore, many projects using MozReview attempt to keep repository history as linear as possible (read: no merge commits) because linear history is easier to reason about and makes operations like bisect simpler.

<span style="text-align:right; font-size: 1.25em; font-style: italic; display:block;">

--[The Docs](http://mozilla-version-control-tools.readthedocs.io/en/latest/mozreview/commits.html#no-merge-commits)

</span>

<div class="img-center">

![No merge commits](/content/images/2016/07/no-merge-commits-1.jpg)
</div>

After checking in with my mentor and another Firefox dev, I learned the solution is to use `hg rebase` instead. The command below is the one they suggested I use:

```
hg pull fx-team && hg rebase -r pending -d fx-team
```

where `pending` is an entry for `[revsetalias]` in my .hgrc file. The entry is below:

```
[revsetalias]
pending= ancestors(.)&draft()
```

For my edification (and yours), I thought I'd break this command down a bit:

```hg pull fx-team```
This is a request to pull in changes from the fx-team remote repository.

```hg rebase```
This command is really interesting. According to `hg help rebase`,

>Rebase uses repeated merging to graft changesets from one part of history (the source) onto another (the destination). This can be useful for linearizing *local* changes relative to a master development tree.

Since this creates a more linear history, I believe this is why MozReview prefers rebasing over merge commits. 

```-r pending` or `-r ancestors(.)&draft()```

The `-r` flag specifies a changeset (and its descendants) to rebase. According to `hg help revsets`, `ancestors()` shows "a greatest common ancestor of the changesets", and the changeset specified is passed in as a parameter to ancestors. I believe the "." specifies the changeset I'm currently working in. Likewise `draft()` references the "changeset in draft phase," ie the unpublished one on my local computer. So all together, these flags are specifying that `hg rebase` should use the greatest common ancestor of the unpublished changeset I'm currently working with. If I misunderstood that, perhaps someone can let me know in the comments.

### Mystery 2: Line changes I didn't make and couldn't remove

Another issue I struggled with recently was line changes being added to a file that I had never expressly meant to add. I would change one line to browser.dtd, but `hg diff` would show that tons of lines had been changed. The weird part was that it didn't look like any edits had been made to the text on the changed lines. 

<div class="img-center">

![hg diff](/content/images/2016/07/hg-diff-1.png)
<i>The results of `hg diff`. The issue seems obvious in hindsight ...</i>
</div>

It turned out my text editor (SublimeText) was trimming trailing white spaces on save, and of course this change was being tracked by Mercurial and being pushed as an unintended part of my patch. 

In the past I had received some feedback about removing trailing white spaces, so I thought I would be clever and set my `trim_trailing_white_space_on_save` preference to true. Set it and forget it! Unfortunately, I had forgotten a little too thoroughly and struggled with this issue for a long time. Checking in with #fx-team on IRC was helpful though. They clued me into the possibility that I had this setting set. 

I mentioned this issue to a A friend of mine, and told me about a SublimeText package that highlights trailing white spaces instead of deleting them. 

![trailing spaces package](/content/images/2016/07/trailing-spaces.png)

Simply enough, it's called ["TrailingSpaces"](https://github.com/SublimeText/TrailingSpaces) and seems like it will be a good solution to noticing trailing white spaces, but not deleting them if I don't want to. 

***

All of this stuff seems sort of obvious now, but hopefully this will be helpful to others learning Mercurial. If you have any other suggestions, please be sure to let me know in the comments. 