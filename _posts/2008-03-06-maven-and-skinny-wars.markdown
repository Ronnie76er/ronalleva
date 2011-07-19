---
author: admin
date: '2008-03-06 10:38:51'
layout: post
slug: maven-and-skinny-wars
status: publish
title: Maven and skinny wars
wordpress_id: '7'
categories:
- maven
---

As I explained in this
[post here](http://www.ronniealleva.org/index.php/2008/01/23/using-the-groovy-maven-plugin-to-do-magic/),
we used the magical Maven groovy plugin to check when our EAR gets
to big, and fail the build.  It works great, because now we get
instant notification in our CI environment when something is amiss,
which might get...um...missed.  So our build has been failing
lately, intermittently because the EAR has been too big. The
problem was the latest version of the Maven war plugin
(2.1-alpha-2-SNAPSHOT) changes the way that skinny WARs are created
for packaging in EARs. The documentation hasn't quite made it to
the
[website](http://maven.apache.org/plugins/maven-war-plugin/examples/skinny-wars.html)
yet.  Instead of using <warSourceExludes\> to exclude the JARs that
you do not want, you have to now use <packagingExcludes\>.  It's
described in this bug
[here](http://jira.codehaus.org/browse/MWAR-135?page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel).


