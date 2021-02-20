---
layout: post
title: "Multithreading: Warning lights"
date: 2014-01-21 12:17:35 UTC
updated: 2014-01-21 12:17:35 UTC
comments: false
categories: [Developer, Jobs]
---

A few days ago I went to an interview which included a pair programming session. The session was a great experience, but the result wasn't quite as good as it should have been, because of some difficulties understanding on what was being asked. It didn't help it was my first experience working with a Mac.
<br /><br />
At one point we agreed on creating a new constructor so that we could pass a service in the constructor instead of getting directly in the method under test.
<br /><br />
So I had something like (fictitious code)
<br /><br />
<blockquote class="tr_bq">&nbsp; class GreatObject {<br />&nbsp; &nbsp; public void calculateAndAddAmount(int length,<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; boolean cheapService) {<br />&nbsp; &nbsp; &nbsp; int cost;<br />&nbsp; &nbsp; &nbsp; if (cheapService) {<br />&nbsp; &nbsp; &nbsp; &nbsp; cost=length*3;<br />&nbsp; &nbsp; &nbsp; } else {<br />&nbsp; &nbsp; &nbsp; &nbsp; cost=length*8;<br />&nbsp; &nbsp; &nbsp; }<br />&nbsp; &nbsp; <br />&nbsp; &nbsp; &nbsp; Service supaService= MegaFactory.getService();<br />&nbsp; &nbsp; <br />&nbsp; &nbsp; &nbsp; supaService.addCost(cost);<br />&nbsp; &nbsp; }<br />&nbsp; }</blockquote><br />and we had agreed on doing something like
<br /><br />
<blockquote class="tr_bq">&nbsp; class GreatObject {<br />&nbsp; &nbsp; private Service supaService;<br />&nbsp; &nbsp; public GreatObject () {<br />&nbsp; &nbsp; &nbsp; this.supaService= MegaFactory.getService();<br />&nbsp; &nbsp; }<br />&nbsp; &nbsp; public GreatObject (Service supaService) {<br />&nbsp; &nbsp; &nbsp; this.supaService=supaService;<br />&nbsp; &nbsp; }<br />&nbsp; &nbsp; <br />&nbsp; &nbsp; public void calculateAndAddAmount(int length,<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; boolean cheapService) {<br />&nbsp; &nbsp; &nbsp; int cost;<br />&nbsp; &nbsp; &nbsp; if (cheapService) {<br />&nbsp; &nbsp; &nbsp; &nbsp; cost=length*3;<br />&nbsp; &nbsp; &nbsp; } else {<br />&nbsp; &nbsp; &nbsp; &nbsp; cost=length*8;<br />&nbsp; &nbsp; &nbsp; }<br />&nbsp; &nbsp; <br />&nbsp;&nbsp;&nbsp; &nbsp; supaService.addCost(cost);<br />&nbsp; &nbsp; }<br />&nbsp; }</blockquote><br />When I was going to do it, something in the back of my mind started to blink. It was a bit subtle and it took me a while to be able to express it. So as an exercise for the future, I'm going to try and express it in writing.
<br /><br />
The problem with the change is that we have transformed a local variable into an instance variable. Local variables are local to the thread. Instance variables are shared between threads. So until now we had a thread safe class, but now the class is not thread safe. As an example: What if each instance of the service can only be invoked once and we have <a href="https://twitter.com/nedbat/status/194452404794691584">two threads</a> running?
<br /><br />
I was asked to tell what to do to be protected. I replied I didn't know. The typical answer involves the reserved word <b>synchronized</b>. But as <a href="http://gonfva.blogspot.co.uk/2012/03/synchronized-silver-bullet.html">I've told in the past</a>, that is not usually a good answer and almost never the best answer.
