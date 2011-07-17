---
author: admin
date: '2010-03-01 02:02:15'
layout: post
slug: materialised-views-and-spring-dao-tests
status: publish
title: Materialised views and Spring DAO Tests
wordpress_id: '131'
categories:
- programming
tags:
- DB
- testing
---

So, you have a materialized view in your DB.  That's great.  Give
yourself a cookie. You also have created a set of tests in your DAO
layer, using the
[AbstractTransactionalSpringContextTests](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/test/AbstractTransactionalSpringContextTests.html). 
Good for you, again. But when you try to mix these two together,
you may see tests failing and can't immediately see why.  This can
happen if you try to get data from the materialized view after
attempting to save data to a table that the view accesses. Well,
it's very simple, and probably pretty obvious.  It wasn't
immediately obvious to me why it was failing, and I didn't really
see anything online about it, so hopefully this will stop your
search quickly. First thing to realize is that running your DAO
tests through the AbstractTransactionalSpringContextTests is that
every test is run in a transaction.  That's great most of the
time.  Each test will run in a little isolated environment and the
changes to the database will be rolled back at the end. You don't
have all kinds of test data strewn about afterwards as if it were
the island in Cast Away. The second thing to realize is that a
materialized view will only be updated on commit.  There's a lot
more to it than that, but I am not a DBA and that's all you need to
know in general. So, with those two things in mind, you can see how
it might be a problem if you try to write tests against code that
uses the materialized view.  All is not lost however.  If you put
an **endTransaction()** call somewhere in your test, all of the DB
calls you've made up to that point will be committed.  Also, any
other calls made after that point will also be committed to the
database.  Since that happens, you will be on your own to clean up
after the test as you see fit.  Also, placing an endTransaction
will only affect the test you place it in.  Any other test will
still continue to run and rollback in the normal fashion. What we
ended up doing was making the tests that need to commit to the
database small as possible.  We then wrapped the
**endTransaction()** function in a function called
**setTestToCommit()** (my co-worker
[James'](http://jlorenzen.blogspot.com/) suggestion), so that it
makes more sense to read it, and placed it at the beginning of the
test. There's lots more information about how it works
[here](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/test/AbstractTransactionalSpringContextTests.html).


