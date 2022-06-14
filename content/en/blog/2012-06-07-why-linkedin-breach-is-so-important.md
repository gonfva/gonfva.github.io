---
layout: post
title: "Why LinkedIn breach is so important?"
previous: https://gonfva.blogspot.com/2012/06/why-linkedin-breach-is-so-important.html
date: 2012-06-07T22:00:04
comments: false
categories: Technology
tags:
    - Technology
---


It's widely spread that there has been a security breach in LinkedIn. At least there is a file with passwords in that social network. Even [LinkedIn acknowledges it and suggests it will try to alert users](http://blog.linkedin.com/2012/06/06/linkedin-member-passwords-compromised/) whose passwords have been compromised.


We know that, and we know passwords were not in the clear (they were stored hashed), but they weren't quite protected (they weren't salted). My own password was on the file and had been "unhashed". No it was not "secret" or "alibaba". Mixed letters and numbers. [You can check yours](http://leakedin.org/) and is a relatively safe process. But even if your password was not in the leaked file, it doesn't mean your password is safe.


So we must assume our Linkedin password and user are compromised. We must assume that every other web we were using that same password in, has the same password compromised.


But we must also assume until proved otherwise that other LinkedIn data has been compromised. Billing address or credit card for premium users? I don't have any information, but from a security perspective (typical CERT practice), and until LinkedIn explains exactly what has happened, is entirely possible.


Somebody could say that credit card data will be hashed. Yep. If it's like passwords, hashed but nos salted. And so crackable.


"Houston we've got a problem".
