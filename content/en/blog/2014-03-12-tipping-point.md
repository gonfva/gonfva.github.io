---
layout: post
title: "Tipping point"
previous: https://gonfva.blogspot.com/2014/03/tipping-point.html
date: 2014-03-12T18:54:24
comments: false
categories: [Developer, got ya, Personal]
tags:
  - Developer
---

I'm happy.

I was developing a feature that allows to export from my showcase project [Whendoigo](http://www.whendoigo.co.uk/) to [TripIt](https://www.tripit.com/).

I was using a gem from TripIt. A gem is kind of a library for Ruby (sort of jar for Java). The gem simply encapsulated the API from TripIt. But it was failing and apparently everything on my project was correct. So this time it didn't took me too long to start debugging the actual gem's code.

The exception had traces with ActiveRecord references. ActiveRecord is an ORM, (think Hibernate or iBatis for Java). It's part of Rails. And the gem shouldn't have any dealing with ActiveRecord

The code that was failing was a bit difficult to understand. It uses one of the more powerful features in Ruby called meta-programming, but not with the more typical ways. Trying to put into words, it was creating a class with a dynamic name on the fly, and then instantiating an object of that class.

What happens when the name it is trying to use is already being used in a different part of the application? To avoid that issue, the gem was creating the names with a namespace.

And that should have been all.

But it wasn't. Lots of debugging and some restructuring later I found the problem.

The names were being cached and when asking for the names it was asking for the names with the namespace... or without it.

[Two lines of code](https://github.com/gonfva/tripit_api/commit/95b120e6920344a9ac27cd647a4b0c41a2f8d2c7). A [pull request](https://github.com/tripit/ruby_binding_v1/pull/1). It took me a while.

Not many people would appreciate the reason.

But I'm really happy.
