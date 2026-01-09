---title: Product, technology and magic
date: 2022-05-08T22:00:00Z
subtitle: Creating a new viral app is not easy
categories: [Technology]
tags: [Technology]
---
# New trend

My daughters call me a senior and [boomer](https://en.wikipedia.org/wiki/OK_boomer). I'm more a [GenX'er](https://en.wikipedia.org/wiki/Generation_X), but I pick my battles.

They are finishing their first year at uni, so they are in the perfect spot to all the new trends. Yesterday, they invited me to the more recent app, called [BeReal](https://bere.al/en).

![Header of a BeReal update](/img/Screenshot_20220508-183557.png "It says 10 minutes late but it was not")

For those who haven't heard about it, it's similar to other social photo apps, like Instagram or Snapchat. You take a picture, and share it with friends or simply with the rest of the world.

The difference with this app is that the app tells you **when** to take the picture. Every day, at a random moment (per timezone), the app says it is the time to upload a new photo, and you have **two minutes** to take it.

Once you upload it and the time has passed, you get a feed with what your friends have uploaded.

# Hooked

The restriction (everybody at the same time) is really smart. Being a random time, and only two minutes, you cannot fake or pose two much. The app usually captures both front a back, so that it is _not only you_, but _what you are doing_, with the main focus on the _what_.

The idea is to encourage people to "be real", as opposed to other apps (*cough* *instagram* *cough*) where we are encouraged to show our better selves.

There is also the social effect of seeing your friends in the cafeteria taking their phones out and knowing it is the time.

Very little time spent, but every single day. The way of [forming habits](https://www.amazon.co.uk/gp/product/B00NW01MKM/). Then the engagement and the dopamine will do the rest. Obviously it is a good way to keep in touch with your friends. In a visual way, because its users are part of a visual generation.

# Technology...

From a technology perspective, the app is fascinating. I'm sure there is some engagement along the day, to see the comments from your friends, but by far the **absolute peak** is in those two minutes and a few minutes after.

Millions of people will try to upload a photo. And those pictures need to go somewhere. The app on your phone is going to communicate with a server managed by BeReal, and then that server will do some processing and allow your friends to discover your picture.

But you need **a lot of servers**.

In the late 90s, early 00s (OK, boomer!!!) we had the [slashdot effect](https://en.wikipedia.org/wiki/Slashdot_effect). Basically somebody in a social network would put a link to your page and you would get thousands of people visiting your site... and crashing it, because your server couldn't cope with so many users.
In those years, to do something like BeReal, you would need to buy servers (and buying a server could take weeks), install it in an expensive place (datacenters) and have them most of the day idle waiting for those daily two minutes.

It was really bad business to have so many servers sitting idle most of the time. But also it meant you have to plan for the load you thought you were going to have several weeks in advance.

Things have changed a lot. Now we have cloud computing. You only need to rent to servers when you need them. The rest of the day can be rented by other people. And in a normal company, you don't need to tell the cloud provider beforehand.

# ...when you need magic

However, cloud computing is not a silver bullet.

You would still find it very hard to grow ("scale up") for such a load as a viral app. You cannot simple say "I want to rent 40,000 servers for two minutes and I want them right now". A couple of weeks ago I came upon this [article on scaling containers](https://www.vladionescu.me/posts/scaling-containers-on-aws-in-2022/) (if you are not in engineering, consider containers as servers).

Usually, what you do in those cases is grow before you need it ("warming"). So if you know your peak is going to come 11:43 am, and _you know how many servers_ you will need, you start scaling up your servers in preparation at say 11:15, so that when the load arrives, you have all the servers you need.

But notice that second condition: you need to know your load in advance.

For an app like BeReal in exponential growth, that is really difficult or even impossible to predict. It probably grows tens of thousands of users a day. How many will post tomorrow? How many from the one that posted today will also post tomorrow?
No alt text provided for this image

And hence, _things happen_.

Today, our first experience, both me and my two daughters took the picture. I didn't care too much, so I uploaded the picture in less than a minute from when the notification came in (you can see that I was programming 😊).

![A BeReal screenshot](/img/Screenshot_20220508-171831.png "Obviously with computers")

My daughters, also tried to upload the picture within the time, but closer to the two minute mark. And their pictures took some time to upload. So for one of them, that meant she _officially_ posted 1 minute late. For the other girl, she appeared as you can see at the top of this article: **10 minutes late**.

It might be a one off, but from a business perspective, this is a **massive** failure.

As a user, if it doesn't matter if I post in time because I'm going to appear as late, I will start posting late. And then not posting at all.

Engagement plummets.

# Product and technology connected

Let me reiterate it. This app is **extremely challenging to manage** from a technical perspective. We're probably talking about tens of millions of uploads in two minutes. There is no easy way to solve it.

As a technologist I see a problem, and I cannot avoid thinking what I would do to solve it (queueing, asynchronous processing...)
But alternative solutions have also product implications.

Currently the feed is obvious.

> A feed with the pictures, sorted by the time when the uploaded completed.

But some of the technical solutions would change that. For example. Should a picture that took 23 minutes to upload because the user is hiking in the mountains and it is using 3G or even EDGE appear the same as the other one that got uploaded immediately because it is using WiFi?

Creating a new viral app is not easy.
But it must be fascinating.