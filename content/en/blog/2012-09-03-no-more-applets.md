---
layout: post
title: "No more applets"
date: 2012-09-03 22:06:06 UTC
updated: 2012-09-03 22:06:06 UTC
comments: false
categories: Technology
---

For more than three years I've been managing certificate-based digital signature applications.
<br /><br />
&nbsp;One of the main source of pain always was and is generating the signature on the user browser. As it needs access to the user's keystore, the browser alone didn't have access to it, and was needed some kind of component (either ActiveX, usually a Java applet). The deployment of the component and the debugging of different OS/browser/Java version configurations tends to be time consuming.
<br /><br />
I even was trying to create <a href="https://github.com/crypteasy/crypteasy-applet">my own version of component to help stop reinventing the wheel</a>.
<br /><br />
For all the time lost, the existence of <a href="http://www.w3.org/2012/webcrypto/">a group in the W3C to incorporate crypto to the browser</a> is GREAT NEWS.
