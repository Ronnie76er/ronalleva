---
author: admin
date: 2015-09-19T01:42:56-04:00
layout: post
slug: ruxit-with-play
title: Getting Ruxit to work with Play
---

At [Accolade](http://www.accolade.com), we've been playing around with the [Ruxit](https://ruxit.com/) platform. But one of the first issues you may hit is how to make it identify your [Play](https://playframework.com/) application correctly. There's this [treatise](https://help.ruxit.com/pages/viewpage.action?pageId=8061067) on how to use an environment variable, but what does it MEAN?  Thankfully, I am here to help.

You simply have to set your `RUXIT_CLUSTER_ID` before running your application.  The easiest way to do this is right on the command line!

	RUXIT_CLUSTER_ID=<my-awesome-name> ./<play-application-run-file>

This also works with `screen`:

	RUXIT_CLUSTER_ID=<my-awesome-name> screen -d -m ./<play-application-run-file>
