---
author: admin
date: '2008-08-28 23:44:23'
layout: post
slug: using-git-and-subversion-in-5-easy-steps
status: publish
title: Using Git and Subversion in 5 Easy Steps
wordpress_id: '11'
categories:
- git
- programming
---

I recently wanted to use a distributed version control system to
learn and to try it out, since I have been hearing great things
about it.  The only issue is that my team (and like 1 million other
teams universe wide) currently uses Subversion, and trying to get
them to switch over would be like trying to punch a redwood down.
But! Not all is lost.  You can use git with Subversion.  This can
give you some of the benefits of using a DVCS without having to
change everything you do.  I believe you can do this also with
Mercurial...I went with git because I had read somewhere it had the
best support to working with Subversion. AND RANDOM INTERNET BLOGS
ARE ALWAYS RIGHT. First of all, I will say that I used
[this](http://utsl.gen.nz/talks/git-svn/intro.html) as a guide when
I was working.  It's a great start to doing this stuff. I'll also
assume you can find and install git on your own, since you're
all-grow'd up now.
## Step 1: Creating a local repo

Ok so let's get started.  The first step is to get the subversion
library.  That webpage I gave you gives you four different ways of
doing it, but I decided to go with the fourth option, which is
cloning the entire Subversion repository.  You can do that by
doing:
    git svn clone https://svnserver/project/trunk

Now, you should know that this could take a while.  You might as
well go and eat dinner for the next three days while this
completes.  Just kidding, this took me about an hour or so while I
was working remotely over a VPN, so it's not so bad.  Our project
has about 8000 revisions, and about 230 MBs of files. So you're
probably wondering "But now if I have the whole history, isn't it
going to take like...a bazillion bytes on my disk?"  To that I say
"Non"...it uses compression, and my git repo ended up being about 1
MB bigger than my subversion one.
## Step 2: Create a branch

From there, you can start working in git!  The first thing you'll
probably want to do is create a branch:
    git branch someNeatoBranch

To change to that branch:
    git checkout someNeatoBranch

Now you are working on this particular branch. Note you can do this
in one step by doing:
    git checkout -b someNeatoBranch

which will create it and put you on it to work.
## Step 3: Commit locally

Now you can edit the files to your heart's content.  And commit
locally as often as you like, by doing
    git commit -a

You can commit individual files by specifying them, but -a just
grabs them all.
## Step 4: Commit back to Subversion

Once you're ready to commit back into the main Subversion repo,
first you need to pull all the changes from there, to make sure
there are no conflicts. You can do this by using this command:
    git svn rebase

This will pull down the latest code and apply your code on top of
it, merging if necessary.  Then to commit back to Subversion, you
use:
    git svn dcommit

And you will notice that your files are committed back to the repo
without any more intervention from you!  But where are your
comments?  Well, all the comments from the commits you did locally
will be used to commit to Subversion.  It will also do that many
commits.  So if you did 5 different commits locally, it will turn
into 5 different Subversion commits.  As long as your team members
don't worry about the revision number spirialing up towards the
heavens, it should be ok.
## Step 5: Brunch!

This is a pretty simple workflow to get you started on using git
with Subversion, but it's not even half the story.  There's so much
more powerful stuff with the branching aspect, and I even haven't
tried it yet to share work with my coworkers before commiting to
svn (I'm kinda like a git island at the moment).  But try it out,
and then check out some of the resources below that helped me get
started. Also, you might want to check how my co-worker
[James](http://jlorenzen.blogspot.com/2008/08/me-lovin-git.html)one-upped
me.  He was working on some highly experimental code that he wanted
to share with a teammate, and created a true git repo in
approximately 8 seconds, from existing code. ****
## **Resources**

-   [An introduction to git-svn for Subversion/SVK users and deserters](http://utsl.gen.nz/talks/git-svn/intro.html)
-   [Git - SVN Crash Course](http://git.or.cz/course/svn.html)



