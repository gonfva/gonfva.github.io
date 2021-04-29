---
layout: post
title: "React-native: some context and a few wrinkles"
date: 2015-04-26 11:48:49 UTC
updated: 2015-04-26 11:48:49 UTC
comments: false
categories: Developer
---

<a href="https://facebook.github.io/react-native/">React native</a> is a way to build iOS applications using Javascript. The idea is not radically new, since you are probably aware of PhoneGap/Cordova and others like that. I myself <a href="http://gonfva.blogspot.co.uk/2011/12/my-first-mobile-application.html">played</a> with something similar long time ago.
<br /><br />
The interesting thing is that React native follows the steps on ReactJS, a Javascript library that provides wonderful performance in updating the DOM.
<br /><br />
It also moves towards CSS into Javascript. You may think "What a crap idea moving CSS into JS?". At least I thought that myself, until I watched the below slides.
<br /><br />
<script async="" class="speakerdeck-embed" data-id="2e15908049bb013230960224c1b4b8bd" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script> <br />Besides, one of my colleagues have recently developed a wonderful <a href="http://en.wikipedia.org/wiki/Single-page_application">SPA</a> using React, so &nbsp;I wanted to give a test to get the idea.
<br /><br />
So OK, I followed the instructions, installed the dependencies, created the project and launched it.
<br /><br />
It fails with a red screen showing a lot of errors related to flow. It turns out the version you get by default when following the instructions is flow 0.10.0. And there is some kind of incompatibility. So you need to install flow 0.9.2
<br /><br />
<blockquote class="tr_bq">$ brew unlink flow&nbsp;</blockquote><blockquote class="tr_bq">$ brew install https://raw.githubusercontent.com/Homebrew/homebrew/6ea5842b3b0101d269a3d66bc44456222da82cc6/Library/Formula/flow.rb</blockquote><br />So now you relaunch and you get a wonderful iOS emulator. Hurray!!! Let's tweak. The screen tells you that you should tweak the file index.ios.js.
<br /><br />
You go to your XCode project and you can't find the file anywhere. Where the heck is it?
<br /><br />
It turns out you have to edit the file OUTSIDE of XCode. Which for me is madness.
<br /><br />
Apart from that, the experience is being quite interesting. On thing I like AND dislike is that the components that React Native tells you to use are direct mapping of the iOS ones. Which means there is a full translation from JS to native code.
<br /><br />
Lots of things to learn. But very good performance (it is not a WebView at all).
<br /><br />
So good idea, but some wrinkles pending.