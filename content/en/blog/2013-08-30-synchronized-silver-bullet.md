---
layout: post
title: "The synchronized silver bullet"
date: 2013-08-30T18:38:19
comments: false
categories: Developer
---

I lead a team that is transitioning a 9-year-old internal framework from a somewhat Struts based to Spring IoC and Spring MVC based. However in the meantime we keep hunting old bugs, which needs to dive in the code. In those&nbsp;occasions, from time to time, I find non-thread-safe code. When talking with the team, one of the usual solutions (at least at the&nbsp;beginning), was something like "Let's put <b><i>synchronized</i></b> to the method". I explained why I thought it was a bad decision. But after some of these episodes, I used to think "how is it that people smart and competent in many aspects of Java, people from whom I learn lots day in day out, just don't get it".
<br /><br />
Today I was reading a book. And I've found <a href="http://books.google.es/books?id=Gu8-_b9AN8gC&amp;printsec=frontcover&amp;hl=es#v=onepage&amp;q=syncrhonized&amp;f=false" target="_blank">this</a>&nbsp;:
<br /><br />
<br /><span style="font-family: 'Courier New', Courier, monospace;">You declare this method as synchronized to make it thread-safe.</span><br /><blockquote class="tr_bq"><span style="font-family: 'Courier New', Courier, monospace;">package com.apress.springenterpriserecipes.sequence;<br />public class SequenceGenerator {<br />private String prefix;<br />private String suffix;<br />private int initial;<br />private int counter;<br />public SequenceGenerator() {}<br />public SequenceGenerator(String prefix, String suffix, int initial) {<br />this.prefix = prefix;<br />this.suffix = suffix;<br />this.initial = initial;<br />}<br />public void setPrefix(String prefix) {<br />this.prefix = prefix;<br />}<br />public void setSuffix(String suffix) {<br />this.suffix = suffix;<br />}<br />public void setInitial(int initial) {<br />this.initial = initial;<br />}<br />public synchronized String getSequence() {<br />StringBuffer buffer = new StringBuffer();<br />buffer.append(prefix);<br />buffer.append(initial + counter++);<br />buffer.append(suffix);<br />return buffer.toString();<br />}<br />}</span></blockquote>
<br /><br />
(mine is a newer version, but you get the point).
<br /><br />
Let me put it clear:&nbsp;Synchronized&nbsp;IS NOT A SILVER BULLET. Synchronized makes sure that code is executed only by one thread. But it doesn't make sure that fields(member-variables) used in that code are not modified in between.
<br /><br />
In particular, in this case, you could get a prefix, then other thread modifies the initial, a different thread modifies the suffix, and you get a real mess in your synchronized method.
<br /><br />
Many people could tell me "You're right, but the setters are not supposed to be invoked after the creation of the object". Then I have to tell two things:
<br /><br />

<ol><li>Delete the setters and the default constructor (Spring allows it).</li><li>Don't synchronize a whole method if you only need to be thread-safe the counter++. There is plenty of methods for doing it.</li></ol><br />And how would I do it?. Uhm. That's a whole post itselt, and it would depend on the class&nbsp;behavior.
<br /><br />
By the way. A great book in the topic: <a href="http://www.jcip.net/" target="_blank">Java Concurrency in Practice</a>.
<br /><br />
