---
author: admin
date: '2010-01-24 21:35:08'
layout: page
slug: conky-hudson-status
status: publish
title: Conky Hudson Status
wordpress_id: '90'
---

### What is it?

[![image](http://www.ronniealleva.org/wp-content/uploads/2010/01/conky_desktop_small.png "Conky on my desktop")](http://www.ronniealleva.org/wp-content/uploads/2010/01/conky_desktop_small.png)
The Conky-Hudson plugin uses the power of conky and the whining of
Hudson to create an annoying way to tell when you or your
co-workers have broken the build, all without having to start up a
pesky web browser that you probably never bring up anyway.

[![image](http://www.ronniealleva.org/wp-content/uploads/2010/01/conky_closeup.png "conky_closeup")](http://www.ronniealleva.org/wp-content/uploads/2010/01/conky_closeup.png)

You can see on the left hand side some of the different states of
jobs in Hudson.  In this set up, a toaster means that it's
building, when the toaster is on fire, it means a build is broken,
and when the build is good, it's like buttered toast.

It's based on the
[ConkyForecast](http://ubuntuforums.org/showthread.php?t=869328),
and I even borrowed the parser function from that code.  You can
see that to the left in the top shot there.

The theme for conky that I used above is Cherries Conky.  But we
can get to that later.

### How to Install

**Pre-requites:**

-   Python 2.6.4
-   conky

**1. Download ConkyHudson: [conkyhudson-0.1.tar.gz](http://cloud.github.com/downloads/Ronnie76er/conkyhudson/conkyhudson-0.1.tar.gz)**

**2. Untar in a directory of your choosing**

These files do not extract in a directory of their own!  I suggest
extracting to .conkyhudson

**3. Set up your conkyHudson.template**

**4. Set up conky to use conkyHudson**

This is accomplished by adding the following line to your .conkyrc
in your home directory:

    ${execpi 10 /home/ronnie/.conkyhudson/conkyhudson.py -t /home/ronnie/.conkyhudson/conkyHudson.template}

And that's it!  You're all set!

More information will be at the
[conkyhudson wiki](http://wiki.github.com/Ronnie76er/conkyhudson/). 
Including how to get the Cherries conky look.

Source code:
[http://github.com/Ronnie76er/conkyhudson](http://github.com/Ronnie76er/conkyhudson)
Some starter templates:
[http://github.com/Ronnie76er/conkyhudsonTemplateExamples](http://github.com/Ronnie76er/conkyhudsonTemplateExamples)


