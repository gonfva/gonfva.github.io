---
layout: post
title: "I was sure it was my fault..."
date: 2014-02-27T19:27:04
comments: false
categories: [Developer, got ya]
---

... but it wasn't.


In the last week or so, I've been hitting against a wall because of two different bugs. I was sure that both bugs were my fault. It took me <b>ages</b> (of a scarce spare time)&nbsp;trying different things, searching for similar problems, until in both cases I finally came to an issue in different tools. As I haven't found too much information, I've decided to put it in writing.


<br /><ul><li>First, trying to use Boostrap (a framework for presentation: sort of a CSS template), and using the grid system (a way to organize things in columns, rows and cells), I wasn't able to get things in the same row and they appeared stacked. The issue turned out to be I was using an old Bootstrap version (3.0.0rc1). Upgrading to a newer version (3.1.1) solved the problem</li></ul><ul><li>Second, again using Bootstrap and its icons (glyphs as they call it), I couldn't help but notice that the icons didn't load properly until the mouse was over the icon (hover). The usual answer on the internet for not loading icons in Bootstrap is "check the class" (it doesn't help that class name for glyphs have changed from Bootstrap 2 to Bootstrap 3). The second most typical answer is related to Firefox (I was using Chrome). Finally I managed to remember that the icons in Bootstrap 3 are fonts. Which led me to know there is a [nasty bug hitting Chrome](https://code.google.com/p/chromium/issues/detail?id=336476) stable with the following title "External Font not rendering until forced repaint". Ouch.</li></ul>


By the way. "It must be my fault" is much better than "it's not my fault". But biases are not good, whether one way or another.


