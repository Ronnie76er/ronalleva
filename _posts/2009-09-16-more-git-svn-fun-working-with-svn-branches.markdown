---
author: admin
date: '2009-09-16 00:47:05'
layout: post
slug: more-git-svn-fun-working-with-svn-branches
status: publish
title: 'More git-svn fun: working with svn branches'
wordpress_id: '50'
categories:
- git
- programming
---

So,
[last time](http://www.ronniealleva.org/index.php/2008/08/28/using-git-and-subversion-in-5-easy-steps/)
I detailed using git to work with your svn trunk. But many of you
asked, "But Ralph, my svn repo has many branches and many tags
because developers love branches and tags! How can I work with
that?" Well, since no one reads this blog and no one actually asked
me that, also misspelling my name in the process, let's just
pretend that what I write is relevant. So you want to work with
multiple svn branches (not to be confused with git branches). What
you have to do is clone the svn repository in a different way, like
this:
    $ git svn clone https://somesvnrepo/svn/project -T trunk -b branches -t tags

This should work in most cases. The arguments to the -T, -b, and -t
switches are the paths that the trunk, branches, and tags are in
respectively. Typically, your branches and tags should be what I
put above, but just in case it is different in your case, there you
go.
## Working with your shiny repo

After a short time in which humans achieve interstellar travel, git
finishes cloning, and you have access to the entire svn repository.
Now, let's say you wanted to start working on trunk. Like before,
you should checkout a branch, but this time it's going to use trunk
as it's parent:
    $ git checkout -b BranchOfTrunk trunk

But Lo! You have the entire repo, so you can checkout that branch
from whatever branch you have in subversion.
    $ git checkout -b BranchOfSubversionBranch subversion-branch

Now, you can switch between BranchOfTrunk and
BranchOfSubversionBranch, and you'll get all the code associated
with trunk and subversion-branch respectively. No more checkout of
a branch ever again, wasting precious keystrokes you could use to
post to Facebook. If a new svn branch is created on the server,
make sure to run the following command to see it:
    $ git svn fetch
    . (updating blah blah)
    .
    .
    $ git branch -a

The last command will show you all the branches, local (your own
branches) and remote (every dumb svn branch), for this repository.
NOTE: **git svn fetch** and **git svn rebase** are almost exactly
the same. **fetch** will update the entire git repository with
anything not received from svn. **rebase** is concerned only with
the parent of the branch you have checked out. If you recall last
time, **git svn rebase** was equivalent to **svn update**. Now you
can work with the same workflow as I mentioned before, working,
committing locally, rebasing, etc.
## Merging from subversion branch to trunk? BALDERDASH!

Now, let's say that you want to commit a series of changes into a
subversion branch, but you also want to commit the same changes in
trunk also. What I'm going to show you is the workflow that I go
through most of the time. I think it's the most effective way to
accomplish it in git. First, you can run a fake dcommit.
    $ git svn dcommit --dry-run
    Committing to https://svnserver/svn/project/branches/subversion-branch ...
    diff-tree b64fdc9250bbe04b3246214ade509e0b0d9d912d~1 b64fdc9250bbe04b3246214ade509e0b0d9d912d

The **"--dry-run"** is key here, for two reasons. First, you'll be
able to see what branch you are actually committing to in
subversion. Not a bad idea, since you're working in a git repo that
has all your subversion branches. Second, you'll see a diff tree
with some of the commit hashes, one corresponding to each time you
did a commit locally, in order. You will want to keep the values of
the hash around, because next we are going to **"cherry-pick"**
them into trunk:
    $ git checkout BranchOfTrunk
    Switched to branch "BranchOfTrunk"
    $ git cherry-pick b64fdc9
    Finished one cherry-pick.
    Created commit e4a7971:  Your commit comments here
     12 files changed, 58 insertions(+), 66 deletions(-)

You'll notice I only used the first 7 digits of the hash (b64fdc9)
to do the cherry-pick. That's all you need to use...git will pick
out the revision from just those seven characters. What this does
is pulls the change from the other branch into this branch. If you
have more than one commit, you should do this multiple times, in
the order of the commits. But then you have the things you did on
the branch ready to be checked into trunk! This is the way I
usually work. You may decide that doing a subversion merge is
easier for what you are trying to do (I sometimes do also). I'd
also like to know if there is an easier way to do this. This
doesn't seem the easiest way, but was the easiest way I could find
so far. And I googled for like...20 minutes. Hopefully this
helps...someone. One person, at least.


