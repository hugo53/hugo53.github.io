---
layout: post
title: "Auto Wake Up, Autoplay Favourite Music"
description: "Somedays I remember old days which my father usually opened guitar songs when he woke up while I was sleeping. That was great to hear smooth melody and be woken up by those beautiful rhythm in the morning. That's why I want to auto wake up my Mac and auto play favourite guitar playlist when I'm still sleeping. Then this is how I did."
category: 
tags: []
---
{% include JB/setup %}

Somedays I remember old days which my father usually opened guitar songs when he woke up while I was sleeping. That was great to hear smooth melody and be woken up by those beautiful rhythm in the morning.
 
That's why I want to auto wake up my Mac and auto play favourite guitar playlist when I'm still sleeping. Then this is how I did, thanks to [Sleepwatcher](http://www.bernhard-baehr.de/).

### Install Sleepwatcher
{% highlight bash %}

sudo mkdir -p /usr/local/sbin /usr/local/share/man/man8
sudo cp ~/Downloads/sleepwatcher_2.2/sleepwatcher /usr/local/sbin/
sudo cp ~/Downloads/sleepwatcher_2.2/sleepwatcher.8 /usr/local/share/man/man8

{% endhighlight %}

Test Sleepwatcher

{% highlight bash %}
man sleepwatcher
{% endhighlight %}


### Create AppleScript 
Before create AppleScript, be sure you have at least one playlist on iTunes. If not, go create it. Just a step using iTunes.
Then now, we write a simple script to open iTunes and play a specific playlist automatically, named ```.wakeup```, looks like that:

{% highlight bash %}

#!/bin/bash 

osascript -e "tell application \"iTunes\" to activate" -e "tell application \"iTunes\" to play playlist \"Test1Playlist\""

{% endhighlight %}

Tell Sleepwatcher wakeup configuration file.

{% highlight bash %}

/usr/local/sbin/sleepwatcher --verbose --wakeup .wakeup
{% endhighlight %}

### Configure to launch Sleepwatcher at startup
Using terminal to configure launch Sleepwatcher at startup and load configuration for wake up behavior. 

{% highlight bash %}
ln -sfv ~/Downloads/sleepwatcher_2.2/config/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist ~/Library/LaunchAgents/

launchctl load ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist
{% endhighlight %}

### Setup Energy Saver on Mac

System Preferences > Energy Saver > Schedule > Startup or Wake > Set time

Notice that we need to plug power adapter to use this feature.

##### Woohoo. So now let wake up with beautiful songs every morning.

### Reference
- [Mac OS X: Automating Tasks on Sleep](https://www.kodiakskorner.com/log/258) by Jason Pitoniak.
- [Controlling iTunes from the Terminal](http://hints.macworld.com/article.php?story=20011108211802830)
