---
layout: post
title: "Why LinkedIn breach is so important?"
date: 2012-08-16T22:00:04
comments: false
categories: Developer
---

<br />It's widely spread that there has been a security breach in LinkedIn. At least there is a file with passwords in that social network. Even <a href="http://blog.linkedin.com/2012/06/06/linkedin-member-passwords-compromised/" target="_blank">LinkedIn acknowledges it and suggests it will try to alert users</a> whose passwords have been compromised.
<br /><br />
We know that, and we know passwords were not in the clear (they were stored hashed), but they weren't quite protected (they weren't salted). My own password was on the file and had been "unhashed". No it was not "secret" or "alibaba". Mixed letters and numbers. <a href="http://leakedin.org/" target="_blank">You can check yours</a>&nbsp;and is a relatively safe process. But even if your password was not in the leaked file, it doesn't mean your password is safe.
<br /><br />
So we must assume our Linkedin password and user are compromised. We must assume that every other web we were using that same password in, has the same password compromised.
<br /><br />
But we must also assume until proved otherwise that other LinkedIn data has been compromised. Billing address or credit card for premium users? I don't have any information, but from a security perspective (typical CERT practice), and until LinkedIn explains exactly what has happened, is enterely possible.
<br /><br />
Somebody could say that credit card data will be hashed. Yep. If it's like passwords, hashed but nos salted. And so crackable.
<br /><br />
"Houston we've got a problem".
<br /><br />
<br />
