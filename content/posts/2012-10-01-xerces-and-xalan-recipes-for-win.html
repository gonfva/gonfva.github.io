---
layout: post
title: "Xerces and Xalan: Recipes for the win"
date: 2012-10-01 14:38:05 UTC
updated: 2012-10-01 14:38:05 UTC
comments: false
categories: [Developer, got ya]
---

OK. I have explained <a href="http://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i_18.html">why Xerces and Xalan are a source of problems</a>, and I have explained why <a href="http://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i_29.html">you shouldn't use System.setProperty to resolve</a> them. In this post I wan't to give a few recipes to resolve Xerces and Xalan problems.
<br /><br />
<ol><li>Under no circumstances use System.setProperty anywhere in your application. I know I have already dedicated a post, but I wanted to stress it once more. Sometimes you'll need some specific parser (OC4J had a strange bug that needed a specific parser, and <a href="http://static.springsource.org/spring-ws/site/faq.html#saaj-weblogic10">Weblogic seems to have special needs</a> too). If you need a specific parser, do it at container level (2nd&nbsp; point), at application level (3rd point) or at the specific code (4th point).</li><li>If you have to define any property do it at the start of the container or the server. For example, in Weblogic 10.3.3 in Windows, we had to edit the file startWeblogic.cmd at the domain path, inserting the following line</li><ol><br /></ol>set JAVA_OPTIONS=-Djavax.xml.soap.MessageFactory=weblogic.xml.saaj.MessageFactoryImpl
<br /><br />
Sometimes you don't need to do it at the container/server level. In Weblogic you can use something called <a href="http://docs.oracle.com/cd/E11035_01/wls100/xml/admin.html">XML registry that enable a different parser for a particular Document Type</a>. <li>Sometimes you can define the parser at the application level. For example, in Weblogic you can define a specific weblogic-application.xml with the following text:<br /><blockquote style="color: #1f497d; font-family: 'Courier New'; font-size: 8pt;">&lt;?xml version = '1.0' encoding = 'UTF-8'?&gt;<br />&lt;weblogic-application xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"<br />xsi:schemaLocation="http://java.sun.com/xml/ns/javaee<br />http://java.sun.com/xml/ns/javaee/javaee_5.xsd<br />http://xmlns.oracle.com/weblogic/weblogic-application<br />http://xmlns.oracle.com/weblogic/weblogic-application/1.1/weblogic-application.xsd"<br />xmlns="http://xmlns.oracle.com/weblogic/weblogic-application"&gt;<br />&lt;xml&gt;<br />&lt;parser-factory&gt;<br />&lt;saxparser-factory&gt;org.apache.xerces.jaxp.SAXParserFactoryImpl&lt;/saxparser-factory&gt;<br />&lt;document-builder-factory&gt;org.apache.xerces.jaxp.DocumentBuilderFactoryImpl&lt;/document-builder-factory&gt;<br />&lt;transformer-factory&gt;net.sf.saxon.TransformerFactoryImpl&lt;/transformer-factory&gt;<br />&lt;/parser-factory&gt;<br />&lt;/xml&gt;<br />&lt;/weblogic-application&gt; </blockquote></li><li>When somebody needs a specific property for a factory and is not possible to modify the container or the application, the solution is not to do a System.setProperty (see 1st bullet), but to instantiantiate the specific factory. For exmaple, instead of doing
<br /><br />
<span lang="EN-US" style="color: #1f497d; font-family: 'Courier New'; font-size: 8pt;">import net.sf.saxon.TransformerFactoryImpl;<br />System.setProperty("javax.xml.transform.TransformerFactory",<br />&nbsp; &nbsp; &nbsp; "net.sf.saxon.TransformerFactoryImpl")<br />TransformerFactory tf= TransformerFactory.newInstance();</span>
<br /><br />
you should do
<br /><br />
<span lang="EN-US" style="color: #1f497d; font-family: 'Courier New'; font-size: 8pt;">import net.sf.saxon.TransformerFactoryImpl; <br />TransformerFactory tf=new TransformerFactoryImp();</span></li>
<br /><br />
<li>In the long run, it should be great to use&nbsp;mechanisms&nbsp;to prevent using System.setProperty like this link&nbsp;<a href="http://stackoverflow.com/a/4208150/54256">http://stackoverflow.com/a/4208150/54256</a></li></ol><div><b>Update (2012-10-01)</b>: I see lots of visits to this page. Maybe you should visit&nbsp;</div><div><br /></div><div><a href="http://gonfva.blogspot.com.es/2012/07/xerces-xalan-and-saxon-more-recipes.html">http://gonfva.blogspot.com.es/2012/07/xerces-xalan-and-saxon-more-recipes.html</a></div><div><br /></div><div>too</div>
