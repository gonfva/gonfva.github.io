---
layout: post
title: "My first Chrome extension(II)"
date: 2012-09-14T21:01:31
comments: false
categories: Developer
---

In <a href="http://gonfva.blogspot.com.es/2012/09/my-first-chrome-extensioni.html">last post</a> I wrote why. In this post I want to explain how I wrote my first chrome extension.
<br /><br />
First. The Chrome Extensions&nbsp;<a href="http://developer.chrome.com/extensions/getstarted.html">developer's documentation</a> is quite good.
<br /><br />
Second. A chrome extension is a zip of a folder with an crx extension instead. The folder has to have a particular file called <a href="https://github.com/gonfva/fromGoogle/blob/master/manifest.json">manifest.json</a>. It tells the extension name and the permissions asked. It also points to the main file. Pretty standard in my case.
<br /><br />
The main file, in my case was called <a href="https://github.com/gonfva/fromGoogle/blob/master/background.js">background.js</a>. You should read the documentation to know more about background pages and so on.
<br /><br />
In my case, I attach an event listener to the click of the extension button (browserAction.onclick). The listener gets the url of the current tab, process that url, encode it, get a Google url redirection from it, and ask for it.
<br /><br />
<code>chrome.browserAction.onClicked.addListener(function(tab) {<br />&nbsp;&nbsp;var currentTab=tab.url;<br />&nbsp;&nbsp;var processedURL=processURL(currentTab);<br />&nbsp;&nbsp;var encodedURL=encodeURIComponent(processedURL);<br />&nbsp;&nbsp;var newUrl="http://www.google.es/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=1&amp;cad=rja&amp;sqi=2&amp;url="+encodedURL;<br />&nbsp;&nbsp;chrome.tabs.update(tab.id, {url:newUrl});
<br /><br />
}); </code>
<br /><br />
The processing of the URL do a bit of tweaking for some specific servers. Pretty uninteresting.
<br /><br />
