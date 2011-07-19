---
author: admin
date: '2008-05-11 23:14:12'
layout: post
slug: my-adventures-with-hardy
status: publish
title: My adventures with Hardy
wordpress_id: '8'
categories:
- ubuntu
---

I finally upgraded my computer to Ubuntu 8.04, but unfortunately my
VMWare server didn't work any more.  That led me into a whole bunch
of things that I noticed wrong with my upgrade.  Allow me to
explain the idiocity. First, for some reason, even after upgrading,
I was still booting into the old 7.10 kernel(2.6.22-14). A quick
change to the /boot/grub/menu.lst file cleared that up.  I just
copied one of the other entries and then modified it so that it
pointed to the new kernel (2.6.24-16). Great.  I booted up and
then...had no wireless.  I had forgotten how I set it up before,
until I came across
[this page.](https://help.ubuntu.com/community/WifiDocs/Driver/bcm43xx/Feisty_No-Fluff#head-bc33832c0547766a33c3a84f13f971ca757b2851)
For some reason, Hardy breaks newer Broadcom wireless cards
(BCM4312, rev. 02 to be exact).  But that page has pretty direct
instructions on how to fix your po' computer...that is, if you have
internet...which you probably do, if you're looking at this page.
Great.  Ok now I can install VMWare. I had it installed before, but
something made it get all scared and I suppose it scurried away to
/dev/null for eternity.  Suicide must have been more appealing to
the VMWare. Apparently there are issues with VMWare on Hardy, but
following this page seemed simple enough.  Except when I ran:
    sudo ./runme.pl

It gave me this error:
    A previous installation of VMware software has been detected.
    
    The previous installation was made by the tar installer (version 3).
    
    Keeping the tar3 installer database format.
    
    Error: Unable to execute "/usr/bin/vmware-uninstall.pl.

So great, it left pieces of itself behind, except without a shovel
to clean them up with. This was easily cleared up by removing the
/etc/init.d/vmware directory. After that, everything worked great. 
I was able to open up my old XP instance perfectly.


