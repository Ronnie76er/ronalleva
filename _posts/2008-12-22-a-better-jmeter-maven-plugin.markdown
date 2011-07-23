---
author: admin
date: '2008-12-22 01:38:46'
layout: post
slug: a-better-jmeter-maven-plugin
status: publish
title: A better JMeter Maven plugin
wordpress_id: '20'
categories:
- jmeter
- maven
---

*UPDATE: You can find the information about the JMeter Maven plugin [here](http://www.ronniealleva.org/index.php/maven-jmeter-plugin/)*

Recently my co-worker [James](http://jlorenzen.blogspot.com/)
suggested we automate our tests on our project using JMeter and
gave me his
[webpage](http://jlorenzen.blogspot.com/2008/03/automated-performance-tests-using.html)
about the trials and tribulations he went through to get it set
up.  It linked to a
[wiki page](http://wiki.apache.org/jakarta-jmeter/JMeterMavenPlugin),
in which someone had created a JMeter plugin for Maven.

Well awesome, it shouldn't be too hard to set up.  And it wasn't, I had
the JMeter tests up and running in a short amount of time.  But one
of the problems was that I wanted to set it up in a more generic
way so that we could define different parameters on each of the CI
environments we ran. 

For this task, I first tried using filtering
in maven, which I got working.  But what you end up with is a JMX
file that is basically unusable by default in JMeter. If you ever
wanted to run it again standalone in JMeter, you'd have to modify
the variables in the JMX file to work correctly. Vice versa, if you
set up your test using JMeter, you have to remember to modify it so
that the maven variables you need are in the places you need them.

JMeter already has a system in it where you can define variables in
the JMX files (and give them defaults, fancy la la!).  For example,
say you want to define a variable for a host name.  Inside your
JMeter JMX file, you can do something like this:

{% highlight bash %}
    ${__P(someVariableName, localhost)}
{% endhighlight %}    

Now, you have defined someVariableName to be a variable within your
JMX file.  The second value, in this case localhost, is the default
if no variable override has been given.

The way to override these
variables is with the -J option on the command, in this form:

{% highlight bash %}
    -JsomeVariableName=yourhost.com
{% endhighlight %}

and all the variables called someVariableName will have the value
yourhost.com. Now, with the minor modifications I have made to the
JMeter Maven plugin, you can now define the values for these
variables inside of your pom.xml, like so:

{% highlight xml %}
<jmeterUserProperties>
      <someVariableName>yourhost.com</someVariableName>
</jmeterUserProperties>
{% endhighlight %}
and it is just the same as using -J on the JMeter command line. The
way we're actually going to use it is to combine it with maven
variables, like so:
{% highlight xml %}
<jmeterUserProperties>
      <someVariableName>${ourhostname}</someVariableName>
</jmeterUserProperties>
{% endhighlight %}

So now, we can just do:

{% highlight bash %}
    mvn -Dourhostname=ourspecialhost ...
{% endhighlight %}

and be able to run site specific JMeter tests with only changing
the arguments on the command-line.

I will be putting together a
page in the next day or so with all the libraries needed to set
this up, similar to the wiki page.


