---
layout: post
previous: https://gonfva.medium.com/shut-down-your-servers-15a7b3f6fe20
title: "Switch off your servers"
subtitle: Cloud is embracing scale and failure
date: 2017-01-21T18:00:00
tags:
  - Developer
categories: [Developer]
---

![](/img/1*fvx5Pd2ymQhkin3kJRSdRQ.png)_Werner’s keynote — <https://youtu.be/65unhiJNaok?t=2692>_

A few months ago, I went to [AWS Summit London 2016](http://medium.com/@gonfva/aws-summit-london-2016-a494dd7b0540#.k5sraje8v). One of the takeaways for me was that the cloud is not only the ability to scale up, but also **down**.

And in particular, that you can **shut down** your servers. And you should if they are not used.

I knew the idea wasn’t strictly mine. What I didn’t know until I was preparing this post is that the idea came from Amazon’s CTO.

Take a look at the picture. Werner himself is telling us that you should switch off your servers when you go home.

When I got the office the following day, I suggested killing most of our development environments every night at 6:30 pm and start them again at 6:30 am.

And we implemented it.

And we’ve been shutting down servers at night, for the last six months, in many of our development environments.

And the experience has been overwhelmingly positive.

### Evangelize

However, every time I explain that we kill development every night, I face surprise and in many cases rejection.

For some time I couldn’t understand questions like “cannot you afford the cost?”, “have you considered spot instances?” or ”is it a good practice?”.

Now, when I hear those questions (and, believe me, I hear them a lot), I force myself to remember that yes, it is an obvious idea for me today. But for me it wasn’t obvious 6 months ago.

So I have decided that if I want to stop being considered a _freak_ or a _penny pincher_, it is time to start explaining the advantages. If I want this to be considered a good practice, it’s better to speak publicly about it.

And if it is not a good practice, better to know quickly (so please comment)

### Advantages

#### Reduced cost

In terms of advantages, the most obvious one is cost. If you start shutting down dev at night and during the weekends, you may reduce your costs a lot. 5 days a week, 12 hours are 60 hours a week, instead of the 168.

60 vs 168.

So in terms of costs, you may reduce your compute costs quite a bit. Not 75% as Werner mentions. In our case it was like 25%. Which is not a minor issue.

But you implement the changes, and then your cloud expenses keeps going up because you use more resources. And in a few months time you are spending more.

#### Increased reliability

The other big advantage is reliability**.** For me this is the less obvious but bigger gain.

Shutting down your servers makes your system more reliable. Because it forces you to follow other good practices:

- You need to have your whole **infrastructure as code**. Do you want to ssh to your machine and tweak something? Fine (you shouldn’t have ssh, but fine). But do it in code because tomorrow your change is gone. Do you want to change the listening port in a load balancer? Remember to do it , in your CloudFormation/Terraform template because tomorrow it will be gone. Have you been a bad guy and you have done docker exec into a docker container? I won’t tell anyone but if you don’t generate a new container, tomorrow your change will be gone.
- Not only that. Shutting down your servers every night implies you have to **plan for failure**. In particular persistent data, either in terms of snapshots or backups that work. Because if you kill your servers, there’s no time for orderly shutdowns. And you will be testing your restore processes **every single day**.
  In my company, we can afford to completely **destroy** the production environment and recreate it in less than hour. Yep, including automatic restores for the databases. And we test it periodically.

#### Cloud native

I perceive there is a third advantage, in a way related to both cost and reliability. It’s **conceptual**.

Cloud has two main dimensions. One is scale. And the other is failure.

In terms of scale, people think that means you can grow without limit. But we tend not to think about shrinking when we don’t need the resources.

And we tend not to think about failure. On aggregate, cloud may be more reliable than our own datacenters. But cloud is failure.

If you kill your development environment when you are not using it, you are scaling down with your use. And you are also incorporating failure tolerance in your system.

And that makes you start thinking in a different way.

I noticed it when I cringed when a colleague was interacting with a Security Group from the web console. It felt _unnatural_.

But the different way of thinking is pervasive.

- You start considering dynamic autoscaling groups both up and down.
- You start considering functional/smoke testing part of your deployment process.
- You start chasing that small bug that only happens for the first user login in a new environment.
- You discover the importance of warming caches.
- You really understand why Database schemas (and migrations) are important and the trade-off in NoSQL.

### Not everything is great

If you’ve read thus far, you may be tempted to think that everything is wonderful and you’re already deciding how to move to this “_environment killing_” world.

And that is great, believe me.

But you may face some challenges too.

- You think your environments are resilient to failure but they aren’t. So in the beginning you will need to set aside some time to chase issues. And for some time your _dev_ environments will be unusable. Factor that time in. Not only the time chasing the bugs, but also the time because of non-usable development environment.
- From time to time, you will face random issues restoring back your servers. You may be tempted to dismiss them as a one off, fix the issue but not the underlying cause. Don’t. It will come back to bite you.
- Some bugs only appear when the environment is running for some time. If you shutdown development and preproduction, but production runs continuously, you may face bugs in production that you haven’t seen anywhere else. There is a balance.
- Shutting down servers needs to be a shared effort. You need to really involve everybody. When part of the team keeps their environment running at night your devops culture is no longer shared.

### Final notes

There are many people that have moved to the cloud but keep thinking in terms of datacenters. You create servers, you patch them, you even [take care of them](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/). Yes, you use Configuration Management Tools (Chef, Ansible…) to create infrastructure and to manage it. But the creation scripts are not frequently used.

Dare to kill your servers every night.
