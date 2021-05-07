---
layout: post
previous: https://medium.com/hackernoon/letsencrypt-paypal-and-selling-fear-21940474d387
title: "LetsEncrypt, Paypal and selling fear"
subtitle: People don’t understand SSL/TLS
date: 2017-03-30T18:00:00
tags:
  - Technology
categories: [Technology]
---

There is a new [report that associates Let’s Encrypt with phishing](https://www.thesslstore.com/blog/lets-encrypt-phishing/). So lots of [people in Twitter](https://twitter.com/search?q=paypal%20letsencrypt&src=typd) started screaming because they read phishing and [Paypal](https://hackernoon.com/tagged/paypal) in the same sentence.

Let’s [Encrypt](https://hackernoon.com/tagged/encrypt) is a foundation that gives certificates away for free. Not only that. Let’s Encrypt is trying to make installing and renewing a certificate effortless.

I decided to read the report which is more a blog post.

Apparently the argument of the report is that Let’s Encrypt doesn’t do a proper validation before giving away the certificates. So many sites use the services in what appear to be Paypal phishing sites.

But the report doesn’t say that domain name sellers are not doing proper validation before selling a phishing domain.

But here comes the twist. The blog post appears in a site called [hashedout](https://www.thesslstore.com/blog).

![](/img/1*QVMyAAojo9aW3qqaGLAVuw.png)Under hashedout’s icon you can see in small letters “by The SSL store”. It turns out that the blog is from a company that [sells certificates](https://www.thesslstore.com/).

> A company that sells certificates creates a report criticising a foundation that gives certificates away for free.Yes. I know that they are different kind of certificates.

In fact the argument is that the certificates they sell are given only after a proper validation.

Anyway, what kind of certificates do they sell?

![](/img/1*cT5Txj1U0hU8tNcNdqP_QQ.png)Cheap!!!Yeah, a lot of Symantec, Thawte and GeoTrust.

The same Symantec, Thawte and GeoTrust that have been [incorrectly emitting certificates](https://security.googleblog.com/2015/10/sustaining-digital-certificate-security.html) (including extended validation certificates, those that have a name in green too) without proper validation.

The same Symantec, Thawte and GeoTrust that [are going to be distrusted](https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/eUAKwjihhBs%5B1-25%5D) by at least a major browser.

There is a serious problem with PKI.

But guys, **you have a nerve**.

[![](/img/1*0hqOaABQ7XGPT-OYNgiUBg.png)](http://bit.ly/HackernoonFB)[![](/img/1*Vgw1jkA6hgnvwzTsfMlnpg.png)](https://goo.gl/k7XYbx)[![](/img/1*gKBpq1ruUi0FVK2UM_I4tQ.png)](https://goo.gl/4ofytp)

> [Hacker Noon](http://bit.ly/Hackernoon) is how hackers start their afternoons. We’re a part of the [@AMI](http://bit.ly/atAMIatAMI) family. We are now [accepting submissions](http://bit.ly/hackernoonsubmission) and happy to [discuss advertising & sponsorship](mailto:partners@amipublications.com) opportunities.
> If you enjoyed this story, we recommend reading our [latest tech stories](http://bit.ly/hackernoonlatestt) and [trending tech stories](https://hackernoon.com/trending). Until next time, don’t take the realities of the world for granted!![](/img/1*35tCjoPcvq6LbB3I6Wegqw.jpeg)
