---
layout: post
previous: https://gonfva.medium.com/devops-is-so-2010-f9425d06261f
title: "DevOps is so 2010. NoOps is the future"
date: 2015-09-04T18:00:00
tags:
  - Developer
categories: Developer
---

![](/img/1*lM5xY8ZDROmYBYZmSYI3bg.jpeg)Cup cake or fairy cake. It doesn’t matter as long as we know what we meanTechnology moves so quickly that people who work in it are always at risk of being obsolete. You learn something new, thinking it’s going to be the next big thing and five years later it’s old and no longer used.

It’s also a trend that reinforces itself. You need to be in the know if you want to stay somehow relevant. So there is some kind of continuous looking of the coolest newest trend.

When I read the title of this post in a [tweet](https://twitter.com/joewalnes/status/639593769852125184), I thought “Ha. Joe Walnes strikes again”

> [](https://twitter.com/joewalnes/status/639593769852125184)He sometimes [uses irony](https://twitter.com/joewalnes/status/631950152006279168). So I was [not the only one considering it a new joke](https://twitter.com/joelash/status/639616612748906496).

But then I started thinking about it. What if it wasn’t a joke?

DevOps is one of the buzzwords nowadays in the technology world. Mix it with the word “Agile”, and your more lo(ath|v)ed recruiter will get absolutely ecstatic.

Is it really DevOps disappearing?

I’m not sure of what Joe Walnes had in mind but here are my thoughts.

#### Dev vs Op

Before DevOps, you used to have Development on one side, and Operations on the other. Developement would create software. Operations would take care of the servers (infrastructure in general) were that software would run. Some companies would have a third leg called QA to assess quality of the software (but let’s be honest, only big companies had one)

That division was, (and still IS in many big companies), part of the lay of the land. Devs weren’t allowed to interfere with Ops. Having two separate roles, allowed for each role to really improve their skills, and it would also make more difficult to appear some security issues.

However that division (and in many times rivalry) was a big problem. Both teams would pass things along but they wouldn’t work together. The lack of real communication was a real issue that made a hassle any change to production.

#### DevOps

With the advent of [agile paradigm](http://www.agilemanifesto.org/principles.html), it was clear that the traditional division of Devs and Ops couldn’t continue in the future. You need to [deliver software frequently](https://www.facebook.com/notes/kent-beck/taming-complexity-with-reversibility/1000330413333156) and requirements, (and usage) are going to change quickly. You cannot use forms as a substitute of real communication. You need to have pure “developers” and pure “operations” working together on the same team deciding how to build and where to deploy the product. How to integrate with other systems. When to bring more servers/instances. What to put in each instance…

From that vision of DevOps of pure Devs and pure Ops working on the same team, we passed to a different vision where there would be people that knows about Development AND Operations. They are no longer Devs or Ops. They are DevOps. Those roles have special value in young startups, where it’s not uncommon to enjoy doing some mounting tables DIY ☺.

#### NoOps?

_“DevOps is cool. We still need Operations.”_

Well, maybe.

Do you remember Samba/Windows Server? When was the last time you had to install a server to [share files](https://www.dropbox.com/en/) among your team?

Do you remember Postfix/Exchange? When was the last time you configured an [email server](https://www.google.com/work/apps/business/)?

_“OK, you’re talking about SaaS. Yeah, we no longer need Operations for that. But if I’m building a software based startup, I surely need some servers”_

[Hell, no](http://www.informationweek.com/cloud/platform-as-a-service/netflix-completes-its-cloud-journey/a/d-id/1321800).

_“I mean, maybe I can use cloud computing. But I surely need to deploy somewhere. And I will need to setup those servers”_

This is for me the crucial part. I think a startup should focus on deliver quickly and test the market. This is part of a bigger topic and I’ll leave it for another post.

But based on something I read somewhere:

> How many startups are building a (bad) Heroku clone not as a product but as part of their deployment process?_“You know, I need to scale, and Heroku is a bit expensive…”_

If you don’t want to use Heroku, fine. Use AWS [Beanstalk](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html). Or any other provider. Do you really need more scale than AWS(Heroku) can provide you? Do you really think AWS costs are going to be greater than the time you’re going to devote to build a bad replica?

_“It is not a matter of scale but flexibility. I need several environments”_

[C’mon](https://devcenter.heroku.com/articles/heroku-labs-new-pipelines).

#### Final thoughts

There will be people dedicated to build/operate hardware infrastructure.

But probably not in your small startup.

For many companies we are headed towards a future [without servers](http://nickmchardy.com/blog/2015/09/my-thoughts-about-aws-api-gateway-working-with-aws-lambda).
