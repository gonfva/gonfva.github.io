---
title: "More Mantis problems"
date: 2010-03-29T11:45:46
categories:
  - Developer
  - got ya
---

I have been fighting with PHP, Mantis and SOAP since [my last post](2010-03-09-mantis-on-sql-server). Now I think I have discovered a
[bug when uploading via Mantis SOAP a binary file](http://www.mantisbt.org/bugs/view.php?id=11722). It's great being able to debug a PHP application. I hope what I discovered is useful for more people. But I've discovered something quite shocking:

I wasn't able to find any good answers to something that must be frequent. I was
uploading a binary file sending nulls: 00 00 00 or when base64-encoded: AAAA (in
hex 41 41 41 41). And I was getting after downloading "\0" (slash-zero) values
(5C 30)
