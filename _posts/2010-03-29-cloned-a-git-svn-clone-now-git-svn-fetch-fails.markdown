---
author: admin
date: '2010-03-29 17:53:50'
layout: post
slug: cloned-a-git-svn-clone-now-git-svn-fetch-fails
status: publish
title: Cloned a git-svn clone, now git svn fetch fails
wordpress_id: '137'
categories:
- git
---

If you go to the bottom of the page for
[git-svn](http://www.kernel.org/pub/software/scm/git/docs/git-svn.html),
under the "Basic Examples" section, you'll see in there a procedure
to clone a git-svn repo that someone has painstakingly already
cloned from svn.  That's great, because it will save you hours and
hours of mind numbing time that would normally take someone to
clone an svn repo.

That person may be tremendously happy...that is,
until, you try to `git svn fetch` to get new revisions and branched
and what have you, and you get an error message like this:
> Last fetched revision of (some branch or tag) was r3421, but we are
> about to fetch: r3421!

In an attempt to fix it, you may find
[this blog post](http://www.jukie.net/bart/blog/20080916155113),
saying to remove the .ref files from your svn directory, which also
proves fruitless.

The fix: modify the `.git/svn/.metadata` file so
that the branches-maxRev and tags-maxRev equals the latest commit
revision in svn.  So it should look like this after you are done:

{% highlight bash %}
; This file is used internally by git-svn
; You should not have to edit it
[svn-remote "svn"]
reposRoot = (svn server name)
uuid = c08781d2-f03e-0410-8c7c-e884ea3e41f3
branches-maxRev = 17224
tags-maxRev = 17224
{% endhighlight %}


Where 17224 was our latest revision.

I got the idea from git-svn's
webpage, where it states:
> If the subset of branches or tags is changed after fetching, then
> .git/svn/.metadata must be manually edited to remove (or reset)
> branches-maxRev and/or tags-maxRev as appropriate.

I must caveat this with the fact that I have no idea why this
works. Git svn is still black magic to me, and making this work is
akin to me throwing muskrat bones onto a flaming pyre, and drawing
conclusions from the charred remains.  Maybe someone with more git
knowledge could drop it off here?


