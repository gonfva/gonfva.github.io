---
layout: post
previous: https://gonfva.medium.com/thoughts-about-s3-outage-b374ed091daa
title: "Thoughts on the S3 outage"
date: 2017-03-01T18:00:00
tags:
  - Technology
categories: [Technology]
---

It’s [in the news](http://www.bbc.co.uk/news/world-us-canada-39119089). Amazon S3, the popular object store crashed yesterday in one of their regions and affected multiple other sites, [including Slack and GitHub](http://venturebeat.com/2017/02/28/aws-is-investigating-s3-issues-affecting-quora-slack-trello/).

![](/img/1*R8FYcp_Jsj5erP9Ws67K_w.png)

So here are some comments about it:

- AFAICT the s3 outage was in us-east-1 region. Other regions didn’t have S3 affected.

  ![](/img/1*2oBaD-BXdtJjRvVuw-dwHw.png)_S3 in Europe* This is [not the first s3 outage](https://twitter.com/chris_stevenson/status/251752053771223040)._
  You may have read about what this affects the 11 nines promise. Most likely, it doesn’t . The promise is for [durability](https://aws.amazon.com/s3/faqs/#data-protection) (if you write, no errors reading), not availability. For availability. The SLA is much lower.
  ![](/img/1*j0tX0qPxNZQNDtxmBZsr7g.png)_SLA is 4 nines.<https://aws.amazon.com/s3/faqs>_

- For quite some time, the [status page in Amazon wasn’t working](https://www.theregister.co.uk/2017/03/01/aws_s3_outage/), affected by their own outage. At some point you could see all green page and only a text at the beginning indicating the outage. Similarly dashboards in many regions wouldn’t work. This is **more serious** than the outage itself.
- If your application is mission critical and cannot afford 3 hours of outage, prepare for it. Go multi region or even multi provider. There are cloud architects designing “high available applications” using three availability zones in a single region. BTW, anything beyond two AZ is flushing money down the toilet.
- If your contingency plan doesn’t allow to deploy without Github, coordinate without Slack, you need to replan.
- Many people are suggesting this goes against cloud or _serverless_. I think this is the very reason you should consider cloud and _serverless_. Managing systems at scale is quite difficult. Leave it to people who know about it (Amazon or other providers)

Final note. I’m super excited about what we will learn from their postmortem.

**Edit**: The postmortem [came out](https://aws.amazon.com/message/41926/). Less interesting than I had thought.
