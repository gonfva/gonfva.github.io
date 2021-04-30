---
layout: post
title: "Things about Xerces, Xalan and Saxon I didn't know (II)"
date: 2012-09-23T11:45:46
comments: false
categories: [Developer, got ya]
---

I said in the previous post that [Xalan and Xerces sometimes look like magic](http://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i.html). So what are they and why are so complicated?.



<ul><li>Xerces is a Java library that helps parsing, validating and managing XML documents. You can parse XML documents in different ways. You can transform into a memory structure, building a big object, or you can even parse only the elements you need. There's a whole array of ways to manage XML documents and schemas, and Xerces helps with them.&nbsp;</li><li>Xalan is a Java library that helps transforming (XSLT 1.0) and querying (XPATH 1.0) XML documents.&nbsp;</li></ul><br />If you've reached this point you'll be asking why the buzz about Xerces and Xalan. There are four things that make Xerces and Xalan special:


<ul><li>Java application servers usually manage their configuration with XML and they need some version of a parser.</li><li>Because of that, Xerces is probably one of more forked projects in the history. Every little server that worth its bucks have its own version of Xerces. Yes, you have one&nbsp;[official&nbsp;version of Xerces](http://xerces.apache.org/#xerces2-j). But your own server maybe has it own. And probably instead of&nbsp;<span style="font-family: monospace; white-space: pre-wrap;">org.apache.xerces.jaxp.DocumentBuilderFactoryImpl</span>, they will have called something like <span style="font-family: monospace; white-space: pre-wrap;">com.sun.apache.xerces.jaxp.DocumentBuilderFactoryImpl</span>.</li><li>Even if your server doesn't use its own version of Xerces, chances are high that it will be using classes of type&nbsp;<span style="font-family: monospace; white-space: pre-wrap;">javax.xml.parsers.DocumentBuilderFactory</span> or&nbsp; <span style="font-family: monospace; white-space: pre-wrap;">javax.xml.parsers.SAXParserFactory</span>&nbsp;to parse xml documents. To get an instance of these classes you'll use a design pattern called factory that will give you a specific implementation of the class. And the specific implementation you'll get depends on... a system property.</li><li>If you need to parse an XML document, you'll have to use those classes too. And if for whatever reason you don't like the specific implementation your server (or even somebody in your own project) is using, then you're basically screwed. Because you'll use something that appears in 80% of the articles about the topic: <b>System.setProperty</b>.</li></ul>
