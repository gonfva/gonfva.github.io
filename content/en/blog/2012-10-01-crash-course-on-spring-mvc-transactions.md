---
layout: post
previous: https://gonfva.blogspot.com/2012/10/crash-course-on-spring-mvc-transactions.html
title: "Crash course on Spring MVC, transactions and persistence (I)"
date: 2012-10-01T19:28:00
comments: false
categories: Developer
tags:
  - Developer
---

A couple of weeks ago I was asked to do an assignment for a selection process. The task was trivial for expected 2-3 hours, and mentioned that the final project would end as a Web Application based on Spring and Hibernate. So I decided to do it as a full application. I didn't want to copy code from work as a template, but I wanted to do it as a fresh project.


If you are busy, go for the code&nbsp;[here](https://github.com/gonfva/assignments/tree/master/gfvQuoteJava).&nbsp;But for those moving from JEE to Spring and wanting a more in-depth explanation, here comes the long (as in two LOOOOOOOOONG posts) detail.
<div>
</div><div>One of the main difficulties was managing the transactions. In particular I had lots of problems with transactions not being persisted, errors like "No Hibernate Session bound to thread, and configuration does not allow creation of non-transactional one here", and&nbsp;NonUniqueObjectException. I wanted to develop with @Transactional. Despite reading the documentation and lots of posts out there, I wasn't able to find explicit end to end functional code that helped me solve my own problems.


Finally I managed to understand what was going on. And I've decided to&nbsp;write my personal <b>crash course on Spring MVC, transactions and persistence</b>. If you arrived here out of the blue, comments are very much appreciated.



<h2><b>
</b></h2><h2><b>Project creation</b></h2>OK. So you want a Spring Web application. Then first thing you should do is create a new web project. There are several alternatives for doing this. In Eclipse the two more obvious are 1.-you can create a Dynamic Web Project, or 2.-create a Maven Project. If you choose the former, you'll have to download and add lots of libraries to the Java Build Path, but you'll have a better understanding about the code. I started from a Maven Web Project. I think I chose a Maven Archetype from codehaus for spring and jboss but I don't really remember because later I tuned it.
Anyway here comes a list of dependencies I have in my project. Some of them come from the maven archetype I chose, so maybe not all are needed:
<ul><li>org.springframework.spring-asm</li><li>org.springframework.spring-aop</li><li>org.springframework.spring-expression</li><li>org.springframework.spring-beans</li><li>org.springframework.spring-context</li><li>org.springframework.spring-context-support</li><li>org.springframework.spring-tx</li><li>org.springframework.spring-core</li><li>org.springframework.spring-jdbc</li><li>org.springframework.spring-web</li><li>org.springframework.spring-orm</li><li>org.springframework.spring-webmvc</li><li>aopalliance.aopalliance</li><li>taglibs.standard</li><li>commons-logging.commons-logging</li><li>log4j.log4j</li><li>org.springframework.spring-asm</li><li>org.springframework.spring-aop</li><li>org.springframework.spring-expression</li><li>org.springframework.spring-beans</li><li>org.springframework.spring-context</li><li>org.springframework.spring-context-support</li><li>org.springframework.spring-tx</li><li>org.springframework.spring-core</li><li>org.springframework.spring-jdbc</li><li>org.springframework.spring-web</li><li>org.springframework.spring-orm</li><li>org.springframework.spring-webmvc</li><li>aopalliance.aopalliance</li><li>taglibs.standard</li><li>commons-logging.commons-logging</li><li>com.h2database.h2</li><li>junit.junit</li><li>org.springframework.spring-hibernate3</li><li>org.hibernate.javax.persistence.hibernate-jpa-2.0-api</li><li>commons-dbcp.commons-dbcp</li><li>commons-pool.commons-pool</li><li>org.hibernate.hibernate-annotations</li><li>jstl.jstl</li><li>org.springframework.spring-test</li><li>javax.transaction.jta</li></ul><h2><b>
</b></h2><h2><b>The <i>web.xml</i></b></h2>As you well know, the file web.xml is essential for every web application as it maps URL patterns to Servlets, among many other things. This is my web.xml file


<script src="https://gist.github.com/3811852.js?file=web.xml"></script>
<div>Things to note. Starting at the end of the file I declare a mapping between one servlet called "Spring MVC Servlet". The servlet, defined in lines 11-17, is of type org.springframework.web.servlet.DispatcherServlet and it takes one parameter named&nbsp;contextConfigLocation. That parameter points to a spring context file named spring-mvc-context.xml (we'll talk about it later).
One point interesting to note is that Spring Web application tend to have two spring-context file. One context-file is related to web (e.g. controllers) and is loaded from the servlet. This is spring-mvc-context.xml.
The other is loaded from a listener. A (servlet) listener acts on container events. In this case we load a &nbsp;listener called ContextLoaderListener (line 9). This listener is in charge of loading the other Spring context. It takes it route from a parameter called&nbsp;contextConfigLocation (lines 5-6).
<b>Warning</b>. You have two context-files [BUT ONLY ONE CONTEXT](http://stackoverflow.com/questions/9227657/web-application-context-root-application-context-and-transaction-manager-setup#comment11620776_9227900). The so called "web context" and "root context" are both part of the same context. You define servlet related things in the "web context" and the rest in the "root context". The web (servlet) context inherits form the "root" context.
So two contextConfigLocation. One for the listener, points in this case to&nbsp;spring-business-context.xml. The other for the servlet, points in this case to&nbsp;spring-mvc-context.xml.
What else? From lines 19-30 we load a filter. A filter, as you well know, is an artifact that sits in between the web and servlet and does something. In this case it is going to do something about transactions, but we'll talk about it later.
<h2><b>
</b></h2><h2><b>The web-scoped context:&nbsp;spring-mvc-context.xml</b></h2>My web context in this case is quite short:


<script src="https://gist.github.com/3811852.js?file=spring-mvc-context.xml"></script> As you can see there are 3 main groups:


<ol><li>context:component-scan looks at the specified package for annotations to fill.</li><li>mvc:annotation-driven [fills some magic](http://stackoverflow.com/questions/3977973/whats-the-difference-between-mvcannotation-driven-and-contextannotation#comment4256810_3978283) relating validation, json and so on.</li><li>InternalResourceViewResolver is a pretty standard way to point to where the JSP reside and the way they are named.</li></ol></div>Nothing very fancy, you see.
<h2>
</h2><h2>The root context: spring-business-context.xml, part I</h2>Ahh. This is quite longer than the web context. Let's see.


<script src="https://gist.github.com/3811852.js?file=spring-business-context.xml"></script> Here you are. In this post the easy part. Next post database, transactions and so on.


First, again, (line 13) a component-scan for annotations in business objects. Beware. If you scan the same package/class in the root context and in the web context, you are screwed and will get some nasty errors.


Second. Line 15. We are going to look for a file called spring.properties (anywhere in the classpath) and we are going to load it. It will help us to separate specific properties from the configuration.


Third. Two bean definitions. Lines 51-56 for one bean, and lines 66-67 for other. The first one needs some properties we just loaded on line 15. The second bean I'm not absolutely sure I need. I think it needs to be here because I have two different beans that could match (one in src and one in test), and here I wanted to load one of them.


That's it. The rest is persistence and transaction related. We'll leave it to the following post.</div><div>
</div><div>
</div><div>
</div>
