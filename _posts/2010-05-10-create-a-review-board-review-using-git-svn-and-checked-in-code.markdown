---
author: admin
date: '2010-05-10 01:45:00'
layout: post
slug: create-a-review-board-review-using-git-svn-and-checked-in-code
status: publish
title: Create a Review Board review using git svn and checked in code
wordpress_id: '150'
categories:
- git
- python
- review board
---

Our group is starting to use
[Review Board](http://www.reviewboard.org/) more and more to
conduct our code reviews. But using it with git-svn, or with
anything you've already checked into svn can be a real hassle.  You
don't want to have uncommitted code hanging around in your computer
for extended periods of time for a number of reasons, so this
rendered it difficult to use for code projects that take more than
a week.

Since I like to eat, and hence get paid so that I can
afford food, I have to work on things that take longer than a
week.  But luckily, git can help get a diff for review board based
only on a string in the commit comments.

This came about when one
of my [co-workers](http://jlorenzen.blogspot.com/) posted a bash
scripts someone had written that would
[take a git diff and make it into a svn-like diff](http://mojodna.net/2009/02/24/my-work-git-workflow.html). 
And that worked great. My first attempt was to modify it to do
multiple files at once, but it ended up making a diff for each
revision for each file (if you need that functionality, you can get
that [here](http://gist.github.com/391247)).

So I pulled out python and worked on a new script, which is at the bottom of this post, or
you can get it [here](http://gist.github.com/395726).

Once you download the file, you run it like this from your git repository:

{% highlight bash %}
    git-create-review.py (some grep string) > diff.output
{% endhighlight %}


The script will take the argument as a string to search for in the
commit comments. This is thanks to the `git log --grep=foo` command
available in git. It outputs directly to the terminal, which you
can see I'm redirecting into diff.output.  Once you have the file,
you can upload to Review Board as you normally would. I did it by
using a search string because we use JIRA to track our work in
svn.  We have pre-commit hooks to ensure that we put a JIRA number
on each commit.  This helps out git as it has something to search
through to get the change list.

**Known issues**:

**First**: It will get all the changes between the first and last revision that
had the search string.

*Example:*

> I change File.java under JIRA-123 (now at r1).  
> Another change to File.java under JIRA-321 (now at r2).  
> I change the file again under JIRA-123 (now r3).  

This script will take all changes between 1 and 3, even if I don't
want the JIRA-321 changes to show up.

**Second**: it doesn't have
any context around removed/moved files. It still should pick up a
renamed file as just a completely new file, but it doesn't have any
context of renaming.  But I believe that's the case with svn
anyway.

**Third**:  I am by no means a good python programmer, and
might not know all the conventions that people normally follow. The
code below may cause eye hemorrhaging, face hemorrhaging...pretty
much any of your organs could be hemorrhaged by looking at the
code. CONSIDER THIS YOUR ONLY WARNING. But I did some testing with
newly created files, and removed files, and binary files, but I'm
sure I missed some crazy edge cases.  Let me know if you hit any
snags. The script follows:


