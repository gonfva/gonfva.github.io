---
layout: post
title: "Notes to a newbie spring-mvc developer"
previous: https://gonfva.blogspot.com/2012/01/notes-to-newbie-spring-mvc-developer.html
date: 2012-01-13T11:45:46
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---


<ol><li>Activate log4j. Without it you're lost</li><li>Look through the log to see if you're getting the RequestMapping through Spring.</li><li>If you are not despite correct annotations, see [this post](http://www.vaannila.com/spring/spring-annotation-controller-1.html). In particular if you've got Spring Security installed, for Goodness sake, don't forget these lines:</li><pre>&lt;beans:bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"&gt;<br/>
&lt;beans:bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"&gt;<br/>
&lt;/beans:bean&gt;<br/>&lt;/beans:bean&gt;</pre><div>
</div><li>Just in case you decided to change the mappings, and you continue to see the old ones in the logs, clean and rebuild.</li></ol>
