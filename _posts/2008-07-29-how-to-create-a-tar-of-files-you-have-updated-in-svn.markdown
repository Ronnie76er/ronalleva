---
author: admin
date: '2008-07-29 18:35:32'
layout: post
slug: how-to-create-a-tar-of-files-you-have-updated-in-svn
status: publish
title: How to create a tar of files you have updated in SVN
wordpress_id: '10'
categories:
- bash
- programming
- ubuntu
---

I had to recently send some modified files to someone so that they
could take a look at what I was doing.  OH NOES!  How do I gather
them all up at once?
    tar cvzf somefile.tar.gz `svn stat | awk '{print "test -f " $2 " && echo " $2}' | bash`

**Word.** To break it down: The svn stat command is getting all the
files that have changed obviously. The awk command its piped to is
the real part that's doing the work.  The awk command prints
    test -f file && echo file

Pipe that through bash, and you have commands, which essentially
will echo the file if the file is a file. Simple? No? I mean, it
will not print it out if it's a directory, so you don't have to
worry about it adding entire directories. Surround that whole thing
in backquotes (that's the one with the tilde (that's the one next
to the '1' key ( I'm not telling you where the '1' key is)))  and
give it as the argument to the tar command.


