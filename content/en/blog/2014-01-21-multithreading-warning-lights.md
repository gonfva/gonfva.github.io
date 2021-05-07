---
layout: post
title: "Multithreading: Warning lights"
previous: https://gonfva.blogspot.com/2014/01/multithreading-warning-lights.html
date: 2014-01-21T12:17:35
comments: false
categories: [Developer, Jobs]
tags:
  - Developer
  - Jobs
---

A few days ago I went to an interview which included a pair programming session. The session was a great experience, but the result wasn't quite as good as it should have been, because of some difficulties understanding on what was being asked. It didn't help it was my first experience working with a Mac.


At one point we agreed on creating a new constructor so that we could pass a service in the constructor instead of getting directly in the method under test.


So I had something like (fictitious code)



<blockquote class="tr_bq">&nbsp; class GreatObject {
&nbsp; &nbsp; public void calculateAndAddAmount(int length,
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; boolean cheapService) {
&nbsp; &nbsp; &nbsp; int cost;
&nbsp; &nbsp; &nbsp; if (cheapService) {
&nbsp; &nbsp; &nbsp; &nbsp; cost=length*3;
&nbsp; &nbsp; &nbsp; } else {
&nbsp; &nbsp; &nbsp; &nbsp; cost=length*8;
&nbsp; &nbsp; &nbsp; }
&nbsp; &nbsp;
&nbsp; &nbsp; &nbsp; Service supaService= MegaFactory.getService();
&nbsp; &nbsp;
&nbsp; &nbsp; &nbsp; supaService.addCost(cost);
&nbsp; &nbsp; }
&nbsp; }</blockquote>
and we had agreed on doing something like


<blockquote class="tr_bq">&nbsp; class GreatObject {
&nbsp; &nbsp; private Service supaService;
&nbsp; &nbsp; public GreatObject () {
&nbsp; &nbsp; &nbsp; this.supaService= MegaFactory.getService();
&nbsp; &nbsp; }
&nbsp; &nbsp; public GreatObject (Service supaService) {
&nbsp; &nbsp; &nbsp; this.supaService=supaService;
&nbsp; &nbsp; }
&nbsp; &nbsp;
&nbsp; &nbsp; public void calculateAndAddAmount(int length,
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; boolean cheapService) {
&nbsp; &nbsp; &nbsp; int cost;
&nbsp; &nbsp; &nbsp; if (cheapService) {
&nbsp; &nbsp; &nbsp; &nbsp; cost=length*3;
&nbsp; &nbsp; &nbsp; } else {
&nbsp; &nbsp; &nbsp; &nbsp; cost=length*8;
&nbsp; &nbsp; &nbsp; }
&nbsp; &nbsp;
&nbsp;&nbsp;&nbsp; &nbsp; supaService.addCost(cost);
&nbsp; &nbsp; }
&nbsp; }</blockquote>
When I was going to do it, something in the back of my mind started to blink. It was a bit subtle and it took me a while to be able to express it. So as an exercise for the future, I'm going to try and express it in writing.


The problem with the change is that we have transformed a local variable into an instance variable. Local variables are local to the thread. Instance variables are shared between threads. So until now we had a thread safe class, but now the class is not thread safe. As an example: What if each instance of the service can only be invoked once and we have [two threads](https://twitter.com/nedbat/status/194452404794691584) running?


I was asked to tell what to do to be protected. I replied I didn't know. The typical answer involves the reserved word <b>synchronized</b>. But as [I've told in the past](http://gonfva.blogspot.co.uk/2012/03/synchronized-silver-bullet.html), that is not usually a good answer and almost never the best answer.
