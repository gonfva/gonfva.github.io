---
layout: post
title: "Xerces, Xalan and Saxon, more recipes"
previous: https://gonfva.blogspot.com/2012/07/xerces-xalan-and-saxon-more-recipes.html
date: 2012-07-01T11:47:46
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

In a previous post I proposed [some tips for trying to manage and control the hell of XML and XPATH parsers](https://gonfva.blogspot.com/2012/01/xerces-and-xalan-recipes-for-win.html). However, as&nbsp;[one reader pointed](https://gonfva.blogspot.com/2012/01/xerces-and-xalan-recipes-for-win.html?showComment=1340120517700#c9002637107572975490), those tips sometimes are not enough. What Simon explained may not seem obvious, but sadly for us (I came across a different problem with the same underlying cause), it is.
<div>
</div><div>In my previous post I explained how to make Java choose a class for a parser. But, unfortunately that's not enough when different libraries provide that class. And that happens to be the case with some classes that are both in Xalan an Saxon (Simon's case) and some Xerces classes (both in server and WEB-INF/lib in my case).&nbsp;</div><div>
</div><div>Simon needs Xalan classes for the general case, but he also needs Saxon for some specific (maybe XPATH2) parsing. He is able to set the parser for the general case, and initialize the specific parser for the XPATH 2 case. But having Saxon jar in the classpath makes the classloader load Saxon classes before &nbsp;Xalan classes.&nbsp;</div><div>
</div><div>What can we do in that case? It's not easy to answer, and sometimes there's not even an answer. Some tips:</div><div>
</div><div><ol><li>Play with the classpath general mechanism loading order. For example JRE/lib/ext are usually loaded before WEB-INF/lib, so maybe Xalan would be placed in JRE/lib/ext and Saxon in WEB-INF/lib. This tip could have it's drawbacks in the booting of the server and in other applications sharing the same JVM, because JRE/lib/ext gets precedence from server specific mechanisms. Besides, operations people gets very nervous about placing anything in the JRE/lib/ext (not without reason).</li><li><span style="background-color: white;">Use some mechanism specific of the server. In my case (Oracle WebLogic Server), there were several</span><ol><li><span style="background-color: white;">Use $DOMAIN/lib folder instead of JRE/lib/ext (ops people get more comfortable, and it reduces the applications affected</span></li><li>Classloader-structure in [weblogic-applicaiton.xml](http://weblogic-applicaiton.xml/).


<span style="background-color: white;"></span></li></ol></li><li>Play with classloaders. [This page from OWLS](http://docs.oracle.com/cd/E15051_01/wls/docs103/programming/classloading.html) is not very server specific and should give some hints.</li><li>In the worst case, I guess one could try to use its one specific packaging.</li></ol><div>Hope this helps. I solved my case but for the general case it will depend on what are we allowed to do.</div></div>
