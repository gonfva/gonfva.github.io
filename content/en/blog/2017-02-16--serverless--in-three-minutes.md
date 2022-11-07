---
layout: post
previous: https://medium.com/hackernoon/serverless-in-three-minutes-9831d5db7a77
title: "“Serverless” in three minutes"
date: 2017-02-16T18:00:00
tags:
  - Developer
categories: [Developer]
---

[Imagine](https://hackernoon.com/tagged/imagine) you are interviewing some guy for an IT role and she says that he doesn’t get that of “_going to the cloud_ because everybody knows there are no datacenters in the _sky_”.

Everybody knows that **_cloud_** in _cloud computing_ is just a name.

However, it’s surprising how many people have a similar idea. You chat with someone in IT about “_serverless_” and she says something like “_Serverless_ doesn’t make any sense. Code has to run on a server”.

![](/img/1*OGTIKrYbATwH2rQaxxGhhw.png)_Quite common opinion_

I understand [technology](https://hackernoon.com/tagged/technology) changes a lot. Sometimes names are not very transparent. And we cannot chase the latest buzzword. Many companies/organisations were still dealing with virtualisation when the cloud came. Then without pause, containers seemed to be the hottest thing.

> “I don’t have time for ‘serverless’, whatever that mean”.So here it comes a three minutes read on the topic.

#### What is serverless?

There is [no easy definition of serverless](https://martinfowler.com/articles/serverless.html).

But you’re busy, so I’ll cut to the chase.

You develop functions[^1]. Small **independent** pieces of code.

We are not talking about static pages or mobile only or single page applications. You develop code for the back-end.

Yes, your code is going to run on servers. But you’re not going to install or configure those servers. For you, it will be as if there are no servers.

> Your code is going to run on servers.
> But you don’t see themIn fact, [your functions are going to run inside containers](https://aws.amazon.com/blogs/compute/container-reuse-in-lambda/), that probably will run on servers. But you don’t care about the servers, nor containers.

You deploy **functions**.

#### Then, why the buzz?

We’ve been deploying packaged applications for 15 years or more. And now that we have containers, we deploy containers without problems.

Why develop in terms of functions? Why the buzz?

![](/img/1*0Agw2zSjrfvyq-VBezij9w.png)_Pancakes…_

For three reasons:

1. With serverless you have “virtually” unlimited scalability. You no longer have to decide how many servers you are going to dedicate to something. Your provider creates as many “function instances” as you need, and destroy them when they are not needed.
2. With serverless, you pay per use. Real use. Not the server up and idle. You pay per code being executed.
3. With serverless you are encouraged to think small and independent.

#### My program is not ‘hello world’!!!

Yes, I hear you screaming.

> “If I only create functions, how the hell am I suppose to do something useful? ’”You know how to create applications, maybe even microservices. How are you supposed to create a full application from functions?

There are [multiple options](http://www.allthingsdistributed.com/2016/06/aws-lambda-serverless-reference-architectures.html).

But you should know serverless works well in both edge cases:

- API endpoint that operates on parameters, query the database and return content. Specially well with [other offerings](https://aws.amazon.com/api-gateway/).
- Event driven or message driven [applications for big data](https://youtu.be/VFLKOy4GKXQ?t=1456).

#### I don’t want to be trapped in AWS

You don’t have to.

All the major providers have a similar technology: [AWS](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html), [Google](https://cloud.google.com/functions/docs/), [Microsoft](https://azure.microsoft.com/en-gb/services/functions/), [IBM](https://console.ng.bluemix.net/openwhisk/).

There is even an Apache project called [Openwhisk](https://github.com/openwhisk/openwhisk) (based on code from IBM)

#### OK, this may be interesting. What now?

Serverless is still evolving. You may want to wait.

It is going to be huge. You may want to do some side project. [Play](https://serverless.com/framework/docs/).

_If you liked this entry, you may also like _[_Switch off your servers_](https://medium.com/@gonfva/shut-down-your-servers-15a7b3f6fe20#.xkftkpisf)_._


[^1]: In this post, Function as a Service (FaaS) and Serverless are synonyms. I know there is some disccussion. Not in this post
