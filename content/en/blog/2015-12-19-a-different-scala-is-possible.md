---
previous: https://gonfva.medium.com/another-scala-is-possible-99bcc6006c7c
layout: post
title: "A different Scala is possible"
date: 2015-12-19T18:00:00
tags:
  - Developer
categories: [Developer]
---

![](/img/1*bLn7IqRLWwwASuEymzeOQg.jpeg) [Scala](http://www.scala-lang.org/), for those who haven’t heard about it, is a programming language. It allows different ways of programming (functional programming but also object oriented programming) It can generate code that runs like, and interoperate seamlessly with, Java.

Scala is getting some traction in the enterprise world because of its features, but lately because it’s used in [Spark](http://spark.apache.org/): the tool of choice in many Big Data projects.

#### My experience

Like many other people in the Scala world, I have some Java knowledge. Almost a couple of years ago, I started working with Scala. From that perspective, there were many things that felt natural: using functions, immutable variables, maps and options. After learning those things, I knew the way I would develop in the future, had changed forever.

However, like many other people, there were other things that were challenging at best. And there was a challenge in the concepts, yes. But also in the way to express those concepts.

For example there is a method called [_foldLeft_](http://www.scala-lang.org/api/2.11.4/index.html#scala.collection.immutable.List@foldLeft[B]%28z:B%29%28f:%28B,A%29=%3EB%29:B). Once you get the concept, you discover it is not very different from a loop with an accumulator using functional programming. But before getting to the concept, you must deal with many issues. You must understand working with functions (fine), you must understand something called curried functions (hmm… fine). And you need to understand that Scala allows to use method names that contain symbols like **/:** (yes, slash colon in this case), and that you won’t be able to look up those method names in Google.

If you keep working with Scala, you will hear non-stop talking about things like monads. What’s a monad? Sort of a special applicative. What’s an applicative? Sort of a special functor. What is functor? Sort of a special typeclass.

You will find out that you’re able to develop software that gets the job done. That you can do it in Scala. But these concepts are still elusive. Even if you’re using monads day in-day out.

I don’t have a software development university degree. So my first feeling was that I was not good enough or that I lacked the background. Later on, I realised that I had been solving (and continued to solve in Scala) problems for my clients/users. So I got away learning enough to be able to understand the documentation.

But, in my case, the feeling “I’m not good enough” persisted for some time.

#### Maybe I’m not alone

The tipping point for me was watching this talk from Rod Johnson

Rod Johnson, for those who don’t know about him, is a very known guy in the Java world. He founded a company called Spring, envisioning that the big paradigm in Java at that moment (EJB2) was a bloated hype. He was right, EJB2 disappeared, and Spring succeed.

In the talk, he mentions some of the topics that were in my head: using a OO approach with Scala is not a sin, the community is not welcoming newbies, maybe we need ways where a newbie can learn Scala slowly (scaffolds or frameworks)

> Maybe I’m not alone on this one. Maybe, it’s perfectly OK to create software that works even if it’s not purely functional.was my feeling.

#### The wave is turning

That feeling has been growing. On a personal level, I’m starting to feel I provide value in solving problems and getting things done. On a Scala community level, I perceive there are more and more people shamelessly expressing similar thoughts.

One relevant example. Last week I was at Scala Exchange 2015 in London. The initial keynote was from [Jessica Kerr](https://skillsmatter.com/skillscasts/6483-keynote-scaling-intelligence-moving-ideas-forward). I don’t like a speaker that reads slides, but I _panic_ when there is talk and it starts with no visual aids (slides) Starting without slides, she managed to capture the attention of us all, with the same ideas as this post but better expressed: go watch the talk.

Jessica is a prolific and very experienced developer. She also comes from Enterprise Java background. But she also has experience in the Ruby community, a friendly community for newbies. In her talk, she perceives there should be a staircase from the green grass of a Scala newbie, to the blue sky of a Scala commercial developer to the outer space of the Scala demigods. And she advocates for a Scala community that generates [good blue sky practices](https://twitter.com/search?q=blueskyscala) for those who can use a map and an option but don’t want to relearn Advanced Algebra to make a web application.

Another relevant example is [Li Haoyi](https://twitter.com/li_haoyi). He’s a core contributor to at least two very interesting Scala projects: [Scala.js](http://www.scala-js.org/) (a port of Scala that generates Javascript) and [Ammonite](https://github.com/lihaoyi/Ammonite) (a shell script with a Scala syntax) He also develops in Python which gives him a unique perspective. And he’s getting increasingly vocal about _smartness_ in the Scala world. See some of his latest tweets.

> [](https://twitter.com/li_haoyi/status/677192563455954944) > [](https://twitter.com/li_haoyi/status/677193847928020992) > [](https://twitter.com/li_haoyi/status/677966448224919554) > [](https://twitter.com/li_haoyi/status/677967362658734080)I perceive there are many more people. People that don’t care if their Scala code is pure functional or not. People that make [useful tools](https://skillsmatter.com/skillscasts/6503-keynote-spark-hadoop-and-how-it-relates-to-scala).

#### Final thoughts

I think it’s not only a problem of better documentation, best practices or frameworks.

I perceive there is some tension between people coming to Scala from a non pure-functional world vs people coming from pure-functional background. The former, often with commercial experience, think Scala is very useful for the kind of software they’re developing, but also feel they don’t need (at least initially) all the mathematical background that is associated to pure functional programming. The latter, often from academia, feel doing things in a non pure-functional way is devaluing the language.

I think everybody has some duty to fill the gap. Better documentation, best practices and frameworks, yes.

And also stepping up and saying it’s OK to work with Scala and feel [**/:**](http://www.scala-lang.org/api/2.11.4/index.html#scala.collection.immutable.List@/:[B]%28z:B%29%28op:%28B,A%29=%3EB%29:B) is a bad name. Or posting tweets saying you use a Java library because the Scala one is not usable.

Or writing a blog post that makes you look stupid.
