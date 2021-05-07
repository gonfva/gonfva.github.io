---
layout: post
title: "No more applets"
date: 2012-09-02T22:06:06
previous: https://gonfva.blogspot.com/2012/09/no-more-applets.html
comments: false
categories: Technology
tags:
    - Developer
---

For more than three years I've been managing certificate-based digital signature applications.


&nbsp;One of the main source of pain always was and is generating the signature on the user browser. As it needs access to the user's keystore, the browser alone didn't have access to it, and was needed some kind of component (either ActiveX, usually a Java applet). The deployment of the component and the debugging of different OS/browser/Java version configurations tends to be time consuming.


I even was trying to create [my own version of component to help stop reinventing the wheel](https://github.com/crypteasy/crypteasy-applet).


For all the time lost, the existence of [a group in the W3C to incorporate crypto to the browser](http://www.w3.org/2012/webcrypto/) is GREAT NEWS.
