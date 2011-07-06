---
layout: post
category: FE
title: use dotjs instead of GreaseMonkey
date: 2011-07-06 19:04:16
---

First, [GreaseMonkey][GM] is too hard to use.

[GM]: http://www.greasespot.net/ "GreaseMonkey"

dotjs is a Google Chrome extension that executes JavaScript files in ~/.js based on their filename.

If you navigate to http://www.google.com/, dotjs will execute ~/.js/google.com.js.

This makes it super easy to spruce up your favorite pages using JavaScript.

Bonus: files in ~/.js have jQuery 1.6.2 loaded, regardless of whether the site you're hacking uses jQuery.

Double bonus: `~/.js/default.js` is loaded on every request, meaning you can stick plugins or helper functions in it.

INSTALL/UNINSTALL:

{% highlight bash %}
git clone git://github.com/hujinpu/dotjs.git
cd dotjs
rake install
# rake uninstall

#output
dotjs
-----
I will install:

1. The 'dotjs' Google Chrome Extension
2. djsd(1) in /usr/local/bin
3. com.github.dotjs in ~/Library/LaunchAgents

Ok? (y/n) y
cp -p bin/djsd /usr/local/bin
mkdir /Users/hjp/.js
chmod 755 /Users/hjp/.js
chmod 644 /Users/hjp/Library/LaunchAgents/com.github.dotjs.plist
starting djdb...
launchctl load -w /Users/hjp/Library/LaunchAgents/com.github.dotjs.plist
Installing Google Chrome extension...
open -a 'Google Chrome' builds/dotjs.crx &
dotjs installation worked
drop files like google.com.js in ~/.js and enjoy hacking the web
{% endhighlight %}

edit qunar.com.js and visit <http://qunar.com> . Note: becareful too many alert.

{% highlight js %}
alert($.fn.jquery); // 1.6.2
{% endhighlight %}

At last, you can keep ~/.js under version control and share it with the world.