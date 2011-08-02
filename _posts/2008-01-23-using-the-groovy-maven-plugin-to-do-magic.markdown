---
author: admin
date: '2008-01-23 12:53:47'
layout: post
slug: using-the-groovy-maven-plugin-to-do-magic
status: publish
title: Using the Groovy Maven plugin to do magic
wordpress_id: '5'
categories:
- groovy
- maven
- programming
---

Recently, we had an issue that came up with creating our EAR for
our web applications. We were trying to package up individual WARs
in an EAR so they could share the same libraries and such and only
load stuff used between them once, to save on memory.

We had it
working great and then something ridiculous happened that caused
WARs with their complete libraries (as opposed to stripped-down
ones) to be included in the EAR, for no apparent reason. This
caused a number of headaches and issues with the application, that
we didn't know until it was deployed, because we failed to
recognize that the EAR had nearly doubled in size.

You may be
wondering, "Hey, I just read two of your dumb paragraphs and I
haven't seen the word 'groovy', 'maven', 'magic', or 'using' yet."
Well, wouldn't it have been nice if we knew instantly that the EAR
had grown, when trying to build? That's where the **using** the
**groovy maven magic** plugin can help!

My co-worker
[James](http://jlorenzen.blogspot.com/) and I attempted to use the
maven enforcer plugin, but it would have taken too much work and we
would have needed to write a custom plugin for it to check the size
of the EAR. So we decided to do it with Groovy. Here's the code:


{% highlight xml %}
<plugin>
  <groupid>org.codehaus.mojo.groovy</groupid>
  <artifactid>groovy-maven-plugin</artifactid>
  <version>1.0-beta-4-SNAPSHOT</version>
  <executions>
    <execution>
      <phase>verify</phase>
      <goals>
        <goal>execute</goal>
      </goals>
      <configuration>
        <source>
 
          def ear = new File("$pom.basedir/target/${project.artifactId}-${project.version}.${project.packaging}")
          log.info("${ear?.length()}");
          def maxsize = project.properties['ear.maxsize'];
          if (ear?.length() > maxsize?.toInteger()) {
            fail("EAR Exceeds maximum size allowed.");
          }
        </source>
      </configuration>
    </execution>
  </executions>
</plugin>
{% endhighlight %}

This beginning part essentially defines the groovy plugin, and
states run the code during the maven's "verify" phase, so we know
that the EAR is already created:
    
{% highlight xml %}
<plugin>
  <groupid>org.codehaus.mojo.groovy</groupid>
  <artifactid>groovy-maven-plugin</artifactid>
  <version>1.0-beta-4-SNAPSHOT</version>
  <executions>
    <execution>
      <phase>verify</phase>
      <goals>
        <goal>execute</goal>
      </goals>
</execution></executions></plugin>
{% endhighlight %}

The groovy code is really straight forward. First we get a
reference to the file:


{% highlight java %}
    def ear = new File("$pom.basedir/target/${project.artifactId}-${project.version}.${project.packaging}");
    log.info("${ear?.length()}");
{% endhighlight %}


We're using variables defined directly in the POM, so we know the
name of the file. We also print the length of the file out, for
sanity's sake. Next we get the max size value:

{% highlight java %}
    def maxsize = project.properties['ear.maxsize'];
{% endhighlight %}


This is defined later in the pom.xml file, under properties:
As you can see, it is set to near 60MBs. Finally, and obviously, we
check the two values against each other, and fail the build if the
EAR is bigger than the size allowed.

{% highlight java %}
    if (ear?.length() > maxsize?.toInteger()) {
      fail("EAR Exceeds maximum size allowed.");
    }
{% endhighlight %}

The `fail()` call in groovy allows you to fail the build with a
message attached to it. So now, if anything fishy happens with the
EAR, we'll be able to know as soon as our CI environment attempts
to build it. This is a really simple example, but you could see how
integrating groovy into maven could allow for some very powerful
things to be done.


