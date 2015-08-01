---
author: admin
date: 2011-11-12 12:00:00
layout: post
slug: conky-hudson
title: Conky Hudson
---
## What is it?


<img src="/images/pages/conky-hudson/conky_desktop_small.png" alt="Woo.  A nice picture of Conky Hudson Status.  Looks awesome." width="100%"/>

The Conky-Hudson plugin uses the power of conky and the whining of Hudson to create an annoying way to tell when you or your co-workers have broken the build, all without having to start up a pesky web browser that you probably never bring up anyway.

<img src="/images/pages/conky-hudson/conky_closeup.png" alt="Look at the burning toasters.  LOOK AT THEM!" width="100%"/>

You can see on the left hand side some of the different states of jobs in Hudson.  In this set up, a toaster means that it's building, when the toaster is on fire, it means a build is broken, and when the build is good, it's like buttered toast.

It's based on the [ConkyForecast](http://ubuntuforums.org/showthread.php?t=869328) setup for conky, and I even borrowed the parser function from that code.  You can see that to the left in the top shot there.

The theme for conky that I used above is Cherries Conky.  But we can get to that later.

## How to Install
### Pre-requites:

* Python 2.6.4
* conky

### Installing

1. Download ConkyHudson: conkyhudson-0.1.tar.gz

2. Untar in a directory of your choosing
    
    These files do not extract in a directory of their own!  I suggest extracting to .conkyhudson

3. Set up your conkyHudson.template

4. Set up conky to use conkyHudson

    This is accomplished by adding the following line to your .conkyrc in your home directory:

{% highlight bash %}
${execpi 10 /home/ronnie/.conkyhudson/conkyhudson.py 
-t /home/ronnie/.conkyhudson/conkyHudson.template}
{% endhighlight %}

And that's it!  You're all set!

More information will be at the conkyhudson wiki.  Including how to get the Cherries conky look.

Source code [on github](http://github.com/Ronnie76er/conkyhudson)

Some starter templates [also on github](http://github.com/Ronnie76er/conkyhudsonTemplateExamples)
