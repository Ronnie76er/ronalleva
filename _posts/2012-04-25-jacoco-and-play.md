---
author: admin
date: 2012-04-25 21:29:00
layout: post
slug: jacoco-and-play
title: Getting code coverage in the Play Framework using Jacoco
---
*QUICKLINK: I don't care about your stupid life story, just [show me how to make it work](#shutup)*

We've been using the [Play](http://www.playframework.org/) framework lately at [KonciergeMD](http://konciergemd.com) to build our application.  One thing that's always good to have on your application is good test coverage, and there's no shortage of tools to measure that.  OR IS THERE?

To get the reports on code coverage, we decided to give [JaCoCo](http://www.eclemma.org/jacoco/) a shot, and specifically try to use the [sbt-jacoco plugin](https://bitbucket.org/jmhofer/jacoco4sbt/wiki/Home) to do it.  

Trying to use it right out of the box, however, proved useless.  Running jacoco:cover from sbt or play would not work, and instead run no tests while not giving any error messages, and making you feel like a fool for trying.  

After staring at the problem for a while, [@seantbrady](https://twitter.com/#!/seantbrady) and I finally came across the solution.  The main and simple issue with it is that sbt runs tests in parallel by default, and this is what the sbt-jacoco plugin inherits.  You can see this by doing the following in the play console:
{% highlight bash %}
[webapp] $ jacoco:parallel-execution
[info] true
{% endhighlight %}

Play, however, has this set to false.  The simple solution, then, is to set this to false for jacoco also.

<a id="shutup">a </a> 

##  How to make it work

Some of this is covered in the [sbt-jacoco wiki](https://bitbucket.org/jmhofer/jacoco4sbt/wiki/Home), but I'm going to put it all here for completeness.

* Add the following to the `plugins.sbt`

{% highlight scala linenos %}
libraryDependencies ++= Seq(
	"org.jacoco" % "org.jacoco.core" % "0.5.7.201204190339" artifacts(Artifact("org.jacoco.core", "jar", "jar")),
	"org.jacoco" % "org.jacoco.report" % "0.5.7.201204190339" artifacts(Artifact("org.jacoco.report", "jar", "jar")))

addSbtPlugin("de.johoop" % "jacoco4sbt" % "1.2.2")
{% endhighlight %}

* We also had to add a `build.sbt` file to the root directory of our project.  It contained the following:

{% highlight scala linenos %}
import de.johoop.jacoco4sbt._
import JacocoPlugin._

seq(jacoco.settings : _*)

parallelExecution in jacoco.Config := false

{% endhighlight %}

The most important thing here is setting the `parallelExecution` to false in the jacoco Config.  

Now you can run `jacoco:cover` to produce your beautiful coverage report! You can see your report at `target/scala-2.9.1/jacoco/html`.
