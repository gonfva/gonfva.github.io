---
layout: post
title: "Little robot (III): Dependency inversion"
date: 2012-08-25T22:11:00
previous: https://gonfva.blogspot.com/2012/08/little-robot-iii-dependency-inversion.html
comments: false
categories: Developer
tags:
    - Developer
---

As I wrote some posts ago, [I received an assignment](https://gonfva.blogspot.com/2012/08/little-robot-i.html), and I decided to do both in Java and in Ruby. When doing in Ruby I wanted to make sure I was able to test the UI, and not only the engine. For doing that, I chose to [fake the console interaction](https://gonfva.blogspot.com/2012/08/faking-console-in-ruby.html) (both stdin and stdout). I didn't consider dependy inversion.


Sometimes a higher level class A has to invoke methods in a lower&nbsp;level class. Let's say this lower level class tends to be class B.&nbsp;Typical approach (non DI) is to make class A depend on class B.&nbsp;Dependency inversion is to make class B implement some contract&nbsp;associated with class A (the dependency is inverted).


I think I'm still in the&nbsp;process of converting from Java(where DI and interfaces is compulsory) to&nbsp;Ruby. With Ruby is much easier to fake classes B and don't even think&nbsp;about inversion while developing class A. &nbsp;Maybe I&nbsp;should have done my class more testable. But I didn't realize it until in a different selection process I was asked when to use Dependency Inversion in Ruby.
