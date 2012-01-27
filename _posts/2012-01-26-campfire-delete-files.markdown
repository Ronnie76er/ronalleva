---
author: admin
date: 2012-01-26 21:29:00
layout: post
slug: campfire-delete-files
title: Delete files from Campfire Rooms
---

I wanted to take a stab at doing some Ruby, so I whipped up this little utility delete all the files in one or all of your campfire rooms for a given URL.

You can get it on [my github.](https://github.com/Ronnie76er/Campfire-Remove-Files)

## Running the program

To run, the command is as follows:
{% highlight bash %}
$ ./campfireremovefiles.rb -s server -t apitoken
{% endhighlight %}

where `server` is the name of your campfire server (in the form of name.campfirenow.com) and `apitoken` is the token given to you.  You can get this token by going under the "My Info" link on your campfire page.

The script will print out a list of rooms, and give you a choice of which one to delete from (or all). You can ignore the "peer certificate" warning, it just means that you're not verifying the cert from campfirenow. We'll assume the server is correct.

Here's what a sample run will look like:

{% highlight text %}
warning: peer certificate won't be verified in this SSL session
Choose the room you want to delete the files from: 
1: Test
2: Test 1
3: Test 2
4: All
? 2
This will delete all files in the room Test 1!!  Are you sure? (y/n)
? y
Deleting test2.txt
Deleting test1.txt
{% endhighlight %}

Again, since this is my first ever Ruby program, it's probably in pretty crappy shape. Deriding comments about the exact level of crappiness are always welcome. 
