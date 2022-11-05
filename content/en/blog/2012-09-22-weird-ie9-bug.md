---
layout: post
title: "Weird IE9 bug"
previous: https://gonfva.blogspot.com/2012/09/weird-ie9-bug.html
date: 2012-09-22T11:09:01
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

This last two weeks I have dealt with a weird bug in IE9.




+ The problem appeared in a legacy web application.
+ The issue reported by the user was a combo that didn't drop down.
+ The problem appeared in different pages, but only for some combination of values.
+ The issue appeared after installing IE9 from IE7 in a Windows Vista.
+ The user could reproduce the issue (for a certain combination of values).
+ We (both devs and ops) couldn't reproduce in a standard machine even tough at work final users deal with a very standard machine (machine setups are pretty standarized at work).
+ When we finally were able to reproduce in a standard machine, it only failed the first time a user logged in (second time, or even IE close rendered the bug irreproducible).
+ We finally were able to pinpoint the problem to a very large option (as in 400 characters).
+ And now I've been able to reproduce it with a reduced version of the page even in my setup (Vista64+IE9). See the gist. Save to a file, launch it with IE9 and try to change the option.
We trimmed down to 200 chars (I guess even 150 chars wouldn't be viewable).


UPDATE (2012-09-30): Tested on IE10 in Windows8 and seems to work fine.


<script src="https://gist.github.com/3763069.js?file=test.html"></script>
