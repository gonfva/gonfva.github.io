---
layout: post
title: "Things about Xerces, Xalan and Saxon I didn't know (III)"
previous: https://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i_29.html
date: 2012-01-29T11:47:46
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

In my previous entry, I tried to explain [why Xerces and Xalan are so complicated]({{< ref "2012-01-18-things-about-xerces-xalan-and-saxon-i_18">}}). In this entry I want to focus in System.setProperty. Because, as I said that is the usual solution you are going to find when you need a specific parser for whatever reason.

And that solution is wrong. Very wrong.

Because, if you choose to change the provider using System.setProperty, you are changing the provider for all the applications in the container/server. It doesn't matter that you change it, then instantiate, and then change to the original value. Because, when you're running in an application server, there are some chances of additional threads needing to instantiate and getting a different provider of the one they are trying to get.

In fact, that same problem was reported to my group a couple of weeks ago.

We tend to give support to applications that use our framework. And, as I say, we received a strange error. Sometimes one report was generated OK, and sometimes it failed with some exception (don't remember off hand).

Same query, same data. Fortunately, the stacktrace came with some reference to xerces. Quick search and discovered the report failed when a different report changed the Property for DocumentBuilderFactory to an old sun implementation. What was even better: the old implementation seemed to be not particularly needed. Looked like a copy&amp;paste.


I guess you'll be asking yourself what to do if you can't use System.setProperty. Too late for this blog post. Wait for the next.


 **EDIT**: Stack trace

```
java.lang.ClassCastException: com.sun.org.apache.xerces.internal.util.XMLGrammarPoolImpl
            at org.apache.xerces.impl.xs.XMLSchemaLoader.reset(Unknown Source)
            at org.apache.xerces.impl.xs.XMLSchemaValidator.reset(Unknown Source)
            at org.apache.xerces.parsers.XML11Configuration.configurePipeline(Unknown Source)
            at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
            at org.apache.xerces.parsers.XML11Configuration.parse(Unknown Source)
            at org.apache.xerces.parsers.XMLParser.parse(Unknown Source)
            at org.apache.xerces.parsers.AbstractSAXParser.parse(Unknown Source)
            at org.apache.commons.digester.Digester.parse(Digester.java:1647)
```
