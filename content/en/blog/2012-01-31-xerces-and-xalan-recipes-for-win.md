---
layout: post
title: "Xerces and Xalan: Recipes for the win"
previous: https://gonfva.blogspot.com/2012/01/xerces-and-xalan-recipes-for-win.html
date: 2012-01-31T14:38:05
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

OK. I have explained [why Xerces and Xalan are a source of problems](https://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i_18.html), and I have explained why [you shouldn't use System.setProperty to resolve](https://gonfva.blogspot.com/2012/01/things-about-xerces-xalan-and-saxon-i_29.html) them. In this post I wan't to give a few recipes to resolve Xerces and Xalan problems.



1. Under no circumstances use System.setProperty anywhere in your application. I know I have already dedicated a post, but I wanted to stress it once more. Sometimes you'll need some specific parser (OC4J had a strange bug that needed a specific parser, and [Weblogic seems to have special needs](http://static.springsource.org/spring-ws/site/faq.html#saaj-weblogic10) too). If you need a specific parser, do it at container level (2nd  point), at application level (3rd point) or at the specific code (4th point).
+ If you have to define any property do it at the start of the container or the server. For example, in Weblogic 10.3.3 in Windows, we had to edit the file startWeblogic.cmd at the domain path, inserting the following line

```
set JAVA_OPTIONS=-Djavax.xml.soap.MessageFactory=weblogic.xml.saaj.MessageFactoryImpl
```

2. Sometimes you don't need to do it at the container/server level. In Weblogic you can use something called [XML registry that enable a different parser for a particular Document Type](http://docs.oracle.com/cd/E11035_01/wls100/xml/admin.html).

3. Sometimes you can define the parser at the application level. For example, in Weblogic you can define a specific weblogic-application.xml with the following text:

```
<?xml version = '1.0' encoding = 'UTF-8'?>
<weblogic-application xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/javaee_5.xsd
http://xmlns.oracle.com/weblogic/weblogic-application
http://xmlns.oracle.com/weblogic/weblogic-application/1.1/weblogic-application.xsd"
xmlns="http://xmlns.oracle.com/weblogic/weblogic-application">
  <xml>
    <parser-factory>
      <saxparser-factory>org.apache.xerces.jaxp.SAXParserFactoryImpl</saxparser-factory>
      <document-builder-factory>org.apache.xerces.jaxp.DocumentBuilderFactoryImpl</document-builder-factory>
      <transformer-factory>net.sf.saxon.TransformerFactoryImpl</transformer-factory>
    </parser-factory>
  </xml>
</weblogic-application>
```

4. When somebody needs a specific property for a factory and is not possible to modify the container or the application, the solution is not to do a System.setProperty (see 1st bullet), but to instantiate the specific factory. For example, instead of doing

```
import net.sf.saxon.TransformerFactoryImpl;
System.setProperty("javax.xml.transform.TransformerFactory",
      "net.sf.saxon.TransformerFactoryImpl")
TransformerFactory tf= TransformerFactory.newInstance();
```


you should do


```import net.sf.saxon.TransformerFactoryImpl;
TransformerFactory tf=new TransformerFactoryImp();
```

5. In the long run, it should be great to use mechanisms to prevent using System.setProperty like this link [http://stackoverflow.com/a/4208150/54256](http://stackoverflow.com/a/4208150/54256)

**Update (2012-10-01)**: I see lots of visits to this page. Maybe you should visit [https://gonfva.blogspot.com.es/2012/07/xerces-xalan-and-saxon-more-recipes.html](https://gonfva.blogspot.com.es/2012/07/xerces-xalan-and-saxon-more-recipes.html) too.
