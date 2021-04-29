---
layout: post
title: "AWS ELB in Apache logs"
date: 2016-09-14T18:00:00
tags:
  - Developer
categories: [Developer, got ya]
---

So you are using AWS Elastic Load Balancer in front of a set of Apache httpd servers and you’ve discovered that your Apache logs are full of garbage. Here comes a set of tips to improve your logging:

- Put a specific endpoint for the health check and don’t log it. ELB needs to check if the endpoint is up and running, so it needs to periodically check an endpoint. By default, all those calls get logged, so better to filter them out.
  RewriteEngine On
  RewriteCond “%{REQUEST_URI}” “^/health/$” [NC]
RewriteRule ^ — [R=200]
SetEnvIf Request\_URI “^/health/$” dontlog
  CustomLog [File] [Format] env=!dontlog* Prevent 408 errors. You may see lots of 408 errors in the logs, and these are caused by ELB. [There are many solutions](http://serverfault.com/a/485129), but if you are going to proxy all the requests, you may simply avoid returning 408.
  RequestReadTimeout header=0 body=0* Don’t use source IP, but forwarding IP. For me this is the big thing. ELB is going to proxy all your requests so you’ll see ELB’s IP addresses as the source of all request. Not very useful. But [ELB provides some headers](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/x-forwarded-headers.html) together with the request. Make use of them, specially of **X-Forwarded-For**.
  CustomLog [File] “%t %{X-Forwarded-For}i %l \”%r\” %>s %b %D \”%{Referer}i\” \”%{User-agent}i\” “I hope these were useful. Feel free to share more similar tips.
