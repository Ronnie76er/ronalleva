---
author: admin
date: '2009-01-12 00:43:35'
layout: page
slug: maven-jmeter-plugin
status: publish
title: Maven JMeter Plugin
wordpress_id: '32'
---

**UPDATE:**The jmeter plugin code has moved to
[github](http://github.com/Ronnie76er/jmeter-maven-plugin)!  Now
you can fork and update to your heart's content. The Maven JMeter
Plugin allows you to automate JMeter tests in Maven.  It was
originally borrowed from
[here](http://wiki.apache.org/jakarta-jmeter/JMeterMavenPlugin "JMeter Wiki")
The JMeter jar was build off of JMeter 2.3.2. Here are the steps
needed to get the JMeter Maven plugin working for you.  This list
is mostly borrowed from
[James' original documentation on the subject](http://jlorenzen.blogspot.com/2008/03/automated-performance-tests-using.html).
**1. [Download](http://jmeter-maven-plugin.googlecode.com/files/jmeter-support-jars-1.0.zip) the support jars and install them.**
Unzip the file, and then use mvn to install the jars in your repo
using the following command (again, thanks
[James](http://jlorenzen.blogspot.com/)).
*mvn deploy:deploy-file -DgroupId=org.apache.jmeter -DartifactId=jmeter -Dversion=2.3 -Dpackaging=jar -Dfile=jmeter-2.3.jar -DpomFile=jmeter-2.3.pom -Durl=file://<repo dir\> mvn deploy:deploy-file -DgroupId=jcharts -DartifactId=jcharts -Dversion=0.7.5 -Dpackaging=jar -Dfile=jcharts-0.7.5.jar**-Durl=file://<repo dir\>*
*mvn deploy:deploy-file -DgroupId=org.apache.jorphan -DartifactId=jorphan -Dversion=2.3 -Dpackaging=jar -Dfile=jorphan-2.3.jar**-Durl=file://<repo dir\>*
*mvn deploy:deploy-file -DgroupId=org.mozilla.javascript -DartifactId=javascript -Dversion=1.0 -Dpackaging=jar -Dfile=javascript-1.0.jar**-Durl=file://<repo dir\>*
**2. [Download](http://jmeter-maven-plugin.googlecode.com/files/maven-jmeter-plugin-1.0.zip) the maven Jmeter Plugin and install it.**
Unzip the file, and then use mvn to install this jar in your repo.
*mvn deploy:deploy-file -DgroupId=org.apache.jmeter -DartifactId=maven-jmeter-plugin -Dversion=1.0 -Dpackaging=jar -Dfile=maven-jmeter-plugin-1.0.jar -DpomFile=**maven-jmeter-plugin-1.0.**pom -Durl=file://<repo dir\>*
**3. Add the dependency to your project pom** Add the following to
your pom:
I'll go over some of the options later.
**4. Create some JMeter tests** This one should be simple enough,
unless you don't know about JMeter.  It can be a bit daunting and
counterintuitive at first, but once you start to get the hang of
it, it will be like second nature, and you will realize how
powerful the tool can be. Once you create your JMeter tests, you'll
want to copy them to <Project Dir\>/src/test/jmeter.
**5. Copy over jmeter.properties** Oh, you do have JMeter, right? 
Else you wouldn't have been able to do step 4.  Copy
<JMETER\_HOME\>/bin/jmeter.properties to <Project
Dir\>/src/test/jmeter.  You can makeany tweats necessary to the
jmeter.properties file you see fit. **6. Run mvn verify** All your
tests should run in maven now! **<jmeterUserProperties\>** In this
section, you can define properties in JMeter files.  For example,
you could define a hostname in your JMeter test like this:
    ${__P(someVariableName, localhost)

Then, in your pom.xml, you can define that variable like so:
    <jmeterUserProperties>
       <someVariableName>hostname.com</someVariableName>
    </jmeterUserProperties>

This has the same effect as using -D on the command line, as
described
[here](http://jakarta.apache.org/jmeter/usermanual/get-started.html)
(search for section 2.4.7). You should now have all you need to run
maven-jmeter-plugin.


