---
layout: post
title: "Scala: First impressions"
previous: https://gonfva.blogspot.com/2014/06/scala-first-impressions.html
date: 2014-06-08T22:05:32
comments: false
categories: Developer
tags:
  - Developer
---

A couple of months ago I started working for a different company. I'm quite happy there for many reasons. But one of them is having the opportunity to learn a new language: Scala. It's uncommon to get a programming job when you don't know the syntax of the programming language used. But if you really think about it, it's much better to hire somebody passionate about technology and that really loves to learn (and commit) than somebody that knows the language but doesn't give a d\*\*\*mn.


Anyway, after a couple of months, I thought it would be good to write my experience about the language.


First. What is Scala? [Scala](http://www.scala-lang.org/) is a programming language that allows to develop both Object Oriented Programming and Functional programming. Functional programming is a new paradigm of developing that is opposed to Imperative programming. In a nutshell, imagine SQL, the language for querying databases. When writing a query you don't write code like "Take the first row, check if satisfies the WHERE clause, if it does, append to the list of results, take the following row...". You simply specify the criteria for the results and that's it.


But that way of querying with SQL that it's inside of almost every developer's brain, involves a new way of thinking about problems for things that are not database queries. And, believe me, it takes time to learn. To the point that today, a couple of months later, and [halfway a course](https://class.coursera.org/progfun-004) devoted to it, I keep writing imperative instead of Functional code.


One could ask "if it is that complicated, why bother?". [Multithreading is tricky](http://gonfva.blogspot.co.uk/2014/01/multithreading-warning-lights.html), but CPUs have stopped increasing speed and they are increasing number of cores, which implies concurrency. Functional programming promises to help in that process. And in fact, one the emerging libraries in the Java field, the [akka library](http://akka.io/), comes from the Scala world. The famous lambda in Java 8 is functional programming.


Regarding the specific language (not the functional paradigm), I have mixed feelings. I find it brilliant sometimes. And others I see it weird. As an example, I love the tuple concept (the ability for a function to return a couple of values at the same time without defining a class for that). But I almost scream each time I have to refer to the first element of the tuple with an \_1 ([as opposed to the typical \_0](http://stackoverflow.com/questions/6241464/why-are-the-indexes-of-scala-tuples-1-based)).


I remember loving Ruby and Rails almost in any line of code. Everything felt natural and although in retrospective there were weird things, my general feeling was that code flows. With Scala I'm just starting to feel productive. With Scala love appears, but it tends to be in "this is freaking awesome" moments from time to time. [OMG, love and code. I'm really a nerd, isn't it?]


Don't get me wrong. Once you get the hang of Scala, it is much much much better than Java. It has a strong typing system like java, but with type inference that allows to skip types in some declarations. It tends to be concise (sometimes to the extreme). In its origin, Scala allowed to generate code both for the JVM and .NET runtime environments. I suspect that's no longer the case and nobody develops in Scala using the .NET.


One thing I find mind blowing (for bad and for good) is implicits. Implicits allow a type to be considered as a different one, upgrading the type to a different one. That is very powerful because the implicit mechanism relies in transforming functions (instead of inheritance or casting). Believe me when I say it is brilliant. But sometimes it leaves you scratching your head thinking why your code does not work until you remember to import the proper implicit conversion function.


The editor would need a post on its own. There is an [Eclipse version for Scala](http://scala-ide.org/). But until last Monday, I wasn't able to do automatic indentation without risking to mangle my code. Refactoring is for brave. And I'm living in nightly updates for Scala-IDE. I've been told that IntelliJ is much better, but I'm trying to avoid it.


Finally, learning a new language is not only the language itself, but a whole array of technologies around. OK. You learn Scala, and then you need to learn a web framework. Play would be the equivalent to the latest versions of Spring MVC or even Rails. But I'm in a love-hate relationship with [lift](http://liftweb.net/)&nbsp;(which I find absolutely different to anything else except for [Meteor](https://www.meteor.com/)). &nbsp;And you also need an ORM. The typical one would be hibernate in the Java world or ActiveRecord for ruby. [Slick](http://slick.typesafe.com/) is not the more used, but it is really promising.


And that's it.


I'm REALLY happy of being able to learn Scala. And I think it has transformed my programming habits for years to come (and it will do more if I leave behind the beginner level).


To the point that [I read val instead of let](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2-XID_1).


I'm not entirely sure I would use Scala for all kind of projects. But certainly I would do for backend mission-critical code.
