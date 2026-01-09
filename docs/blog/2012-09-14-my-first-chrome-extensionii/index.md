---title: My first Chrome extension(II)
date: 2012-09-14T21:02:31Z
categories: Developer
tags: [Developer]
---

In [last post]({{< ref "2012-09-14-my-first-chrome-extensioni">}}) I wrote why. In this post I want to explain how I wrote my first chrome extension.


First. The Chrome Extensions [developer's documentation](http://developer.chrome.com/extensions/getstarted.html) is quite good.


Second. A chrome extension is a zip of a folder with an crx extension instead. The folder has to have a particular file called [manifest.json](https://github.com/gonfva/fromGoogle/blob/master/manifest.json). It tells the extension name and the permissions asked. It also points to the main file. Pretty standard in my case.


The main file, in my case was called [background.js](https://github.com/gonfva/fromGoogle/blob/master/background.js). You should read the documentation to know more about background pages and so on.


In my case, I attach an event listener to the click of the extension button (browserAction.onclick). The listener gets the url of the current tab, process that url, encode it, get a Google url redirection from it, and ask for it.


<code>chrome.browserAction.onClicked.addListener(function(tab) {
  var currentTab=tab.url;
  var processedURL=processURL(currentTab);
  var encodedURL=encodeURIComponent(processedURL);
  var newUrl="http://www.google.es/url?sa=t&amp;rct=j&amp;q=&amp;esrc=s&amp;source=web&amp;cd=1&amp;cad=rja&amp;sqi=2&amp;url="+encodedURL;
  chrome.tabs.update(tab.id, {url:newUrl});


}); </code>


The processing of the URL do a bit of tweaking for some specific servers. Pretty uninteresting.
