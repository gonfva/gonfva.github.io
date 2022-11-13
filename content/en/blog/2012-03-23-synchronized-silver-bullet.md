---
layout: post
title: "The synchronized silver bullet"
previous: https://gonfva.blogspot.com/2012/03/synchronized-silver-bullet.html
date: 2012-03-23T18:38:19
comments: false
categories: Developer
tags:
  - Developer
---

I lead a team that is transitioning a 9-year-old internal framework from a somewhat Struts based to Spring IoC and Spring MVC based. However in the meantime we keep hunting old bugs, which needs to dive in the code. In those occasions, from time to time, I find non-thread-safe code. When talking with the team, one of the usual solutions (at least at the beginning), was something like "Let's put  **<i>synchronized</i>** to the method". I explained why I thought it was a bad decision. But after some of these episodes, I used to think "how is it that people smart and competent in many aspects of Java, people from whom I learn lots day in day out, just don't get it".


Today I was reading a book. And I've found [this](http://books.google.es/books?id=Gu8-_b9AN8gC&amp;printsec=frontcover&amp;hl=es#v=onepage&amp;q=syncrhonized&amp;f=false) :

```
You declare this method as synchronized to make it thread-safe.

package com.apress.springenterpriserecipes.sequence;

public class SequenceGenerator {
  private String prefix;
  private String suffix;
  private int initial;
  private int counter;

  public SequenceGenerator() {}

  public SequenceGenerator(String prefix, String suffix, int initial) {
    this.prefix = prefix;
    this.suffix = suffix;
    this.initial = initial;
  }

  public void setPrefix(String prefix) {
    this.prefix = prefix;
  }

  public void setSuffix(String suffix) {
    this.suffix = suffix;
  }

  public void setInitial(int initial) {
    this.initial = initial;
  }
  public synchronized String getSequence() {
    StringBuffer buffer = new StringBuffer();
    buffer.append(prefix);
    buffer.append(initial + counter++);
    buffer.append(suffix);
    return buffer.toString();
  }
}
```


(mine is a newer version, but you get the point).


Let me put it clear: Synchronized IS NOT A SILVER BULLET. Synchronized makes sure that code is executed only by one thread. But it doesn't make sure that fields(member-variables) used in that code are not modified in between.


In particular, in this case, you could get a prefix, then other thread modifies the initial, a different thread modifies the suffix, and you get a real mess in your synchronized method.


Many people could tell me "You're right, but the setters are not supposed to be invoked after the creation of the object". Then I have to tell two things:




+ Delete the setters and the default constructor (Spring allows it).
+ Don't synchronize a whole method if you only need to be thread-safe the counter++. There is plenty of methods for doing it.

And how would I do it?. Uhm. That's a whole post itselt, and it would depend on the class behavior.


By the way. A great book in the topic: [Java Concurrency in Practice](http://www.jcip.net/).
