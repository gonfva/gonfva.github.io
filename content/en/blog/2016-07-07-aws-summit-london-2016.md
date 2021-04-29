---
layout: post
title: "AWS Summit London 2016"
date: 2016-07-07T18:00:00
tags:
  - Technology
categories: [Technology]
---

Today I went to [AWSSummit London](https://awssummit.london/). It’s a free event dedicated to Amazon Web Services. The event took place at [Excel London](http://excel.london/), excellent venue except for the occasional noise due to planes taking off from the nearby London City Airport.

![](/img/1*rpqnIPjIYSOEGlY_9oIMQg.jpeg)KeynoteThe day started with an intro from Gavin Jackson, expressing that there was a promise from Amazon for a new UK region for late 2016 or early 2017. And that promise continues after Brexit vote, and also the[ general commitment from AWS with UK startup ecosystem](http://www.theregister.co.uk/2016/07/07/amazon_on_brexit/).

#### Keynote

After the brief introduction, a keynote from [Werner Vogels](https://twitter.com/Werner), CTO at Amazon.com. The keynote was a bit long for my taste (10:00 to 12:00), but not because of Werner but because some of the “success stories” from some AWS customers.

Werner’s talk didn’t have anything newsworthy but it was full of little gems. You perceive he is a guy with the concepts well established, so I ended up making some mental notes or even taking pictures, like the below:

![](/img/1*9tK2MnQK0NL_eI4aA8gLaA.jpeg)Some signs you are not at Microservice level yetOne interesting idea: don’t treat VMs as servers. It’s a the very well known paradigm: pets vs cattle. With pets you have a name and you take care of them. Cattle are not pets.

Servers used to be like pets. You used to put names from Star Wars or animals and you would take care, patch, clean and so on. If a server had a disk space full, you would clean temps. But in the cloud, VMsis about embracing failure. A VM fails, you kill it, you don’t try to fix it.

Another idea was about the importance of scaling down. Werner spoke a lot about how he had tried to to introduce the on-demand factor, no-commitments into IT, as a contrast to what he had been suffering (long commitments and contracts with IT providers) In relationship with that, something that has been coming to my mind throughout the day is the ability to scale up… but also down. You pay for what you need. You scale up when the demand is high … and you also should scale down when it passes. Somebody mentioned in one of the talks about shutting down dev environments at night. We don’t shut down envs at night. We don’t scale down environments.

The extreme about on-demand no-commitment is AWS Lambda (I’ll speak about it later)

One (for me) controversial topic was about containers orchestration. There are many competing technologies: [Kubernetes](http://kubernetes.io/), [Docker Swarm](https://docs.docker.com/swarm/), [Mesos+Marathon](https://mesosphere.github.io/marathon/) and [Hasicorp stack](https://www.nomadproject.io/intro/vs/index.html). AWS has its own system, called [ECS](https://aws.amazon.com/ecs/getting-started/), and it also includes its own private registry for containers (with my experience, I can promise that’s a big bonus)

I don’t remember Werner’s own words but he said something like (my memory, no writing) “You see a product that has just released a new version, you think in terms of high availability, and you need ZooKeeper or ETCD to get it. With ECS you have high availability in the system […] I cannot understand anybody using anything different from ECS to deploy containers in AWS in production”. Again. No quotes, just my memories. But, for me, he was pointing to Kubernetes 1.3 new release and the fact that it hasn’t HA backed in the system (I may have misunderstood him)

But in connection to that, it’s been a surprise: no mention in ANY talk about Kubernetes. NO MENTION at all. And lots of mentions to ECS. So it has been controversial for me, but there hasn’t been any controversy.

AWS would win lots of money from people deploying Kubernetes into AWS. AWS doesn’t get any money from ECS. It gets from the machines were the containers run, so comparing apples to apples, it is as if you were getting Kubernetes master free. And Kubernetes seems to be the cool framework (or it used to be, until Docker decided to put a [Trojan house into Docker engine](https://docs.docker.com/engine/swarm/)) Granted, Kubernetes is pushed by Google, and it helps give Google some leverage to its cloud offering. But I’m still a bit surprised. To the point I may have drunk the kool-aid about ECS: I’m planning to do a quick test even on my own time.

Is it that Kubernetes is such a bad offer compared to ECS as they implicitly say? Or is it that AWS don’t want Kubernetes to succeed to stop Google from gaining momentum?

I know people anchored on the hybrid cloud would find difficult to deal with ECS. But for cloud people that deploy to AWS, ECS may be a very good option.

The keynote finished late. That meant for me that I had just the time to move to a different room, with no time to pick the lunch (I had lunch later).

#### Other talks

After the keynote, there were different talks in several different rooms, and you could choose the topics you were most interested in.

The second talk I attended was about [Big Data architectural patterns](https://awssummit.london/session/2016/c6864ede-390d-4477-bc65-ef57bd7c9f24). Not too much to write. As I’ve mentioned, I arrived late, so I didn’t get into the talk. My main takeaway was about distinguishing between cold, warm and hot data, taking into account how fast you need the data, how much it costs, and how much data are you storing. And leverage the different offerings from AWS. I also noted down several mentions about [Apache Zeppelin](https://zeppelin.apache.org/).

[The third talk](https://awssummit.london/session/2016/abb14293-271b-4b45-b52c-ca9b997556ed) I attended was about AWS lambda and “serverless” architecture. I’m a huge fan of the serverless architecture (with no success around me so far), even if I perceive the governance is ([used to be](http://serverless.com/)?) difficult.

First, some obvious message: the serverless part doesn’t mean there is no servers. There are servers, but you don’t care about them. You don’t install, you don’t configure, you don’t scale servers.

Basically AWS lambda is a way to execute code, without having to manage the servers where the code runs. You pay only for seconds of executions. And included free there are 1 million executions and up to 3.2 million seconds a month.

It was an introductory talk to [AWS lambda](https://aws.amazon.com/lambda/details/) and [AWS API Gateway](https://aws.amazon.com/api-gateway/), but even included a demo with a web form served from S3 writing to a DynamoDB database using API Gateway, AWS lambda and created with cloudformation (infrastructure as code). It also revealed something that I didn’t (formally) know: it is based in Linux containers (cgroups and that stuff). Not docker, but similar concept. I should have taken a picture of that slide :(

There was also a very cool case from Paralax a company that uses serverless architecture to deploy super-scalable 3 weeks idea to live projects. Cool

The next two talks were about containers and ECS. The [first one](https://awssummit.london/session/2016/716886b4-1bad-4ec4-9a32-0063e25e88f1) was more introduction. The [second one](https://awssummit.london/session/2016/76f8a463-f30a-455e-b817-d142afaddbd4) a deep dive. The first one enough to encourage me taking a look at ECS. The second one, a bit repetitive since the slides were in many cases the same. Both joint, enough to make me think about taking a look to ECS.

The last talk of the day for me was about [securing serverless architecture](https://awssummit.london/session/2016/c0d57c29-1990-4be8-9a87-2086d596d14a). I would have titled it as hardening serverless architecture, because I was expecting more a mid level talk about execution roles, but ended being a talk about advanced techniques for securing AWS Lambda. There were some interesting takeaways for me, including how to deploy secrets to instances using “serverless”:

![](/img/1*qtral1Pk0R0tgBeStwa03Q.jpeg)Serverless to provision secrets#### Stands and sponsors

There were plenty of stands with different sponsors and even some AWS architects to help you for free.

![](/img/1*AAS6KqsF8xXpum3Ba2_usQ.jpeg)Overview of the networking areaThere was also the usual kind of branded t-shirts and stickers, although more in the morning (before and after the keynote) I got a Google cardboard, but discovered that my phone has no gyroscope so it won’t work.

I would like to point two sponsors.

First [Seldon](http://www.seldon.io/), a company I hadn’t heard about. They have an open source product that helps dealing with Machine Learning . It also “encapsulates” (not quote but meaning) machine learning libraries. As a company they provide enterprise support and also modeling.

The other [Claranet](http://www.claranet.co.uk/). Basically a consultancy company for helping you going cloud.

#### Conclusion

A very interesting event to get an idea of what AWS and other partners are offering. And free (apart from my time). If you use AWS in your company, AWS Summit is a no-brainer.
