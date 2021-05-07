---
layout: post
previous: https://gonfva.medium.com/trivial-error-with-bitbucket-api-db64fe71e379
title: "Trivial error with Bitbucket API"
subtitle: Migrating to cloud Bitbucket
date: 2018-10-13T18:00:00
tags:
  - Developer
categories: [Developer, got ya]
---

**UPDATE**: A few days after publication, [the documentation now includes](https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Busername%7D/%7Brepo_slug%7D/pullrequests#post) some examples. Well done, Atlassian.

Most people in the software industry know [Jira](https://www.atlassian.com/software/jira?). For better or for worse, it’s the most used product for managing tickets, requests, bugs…

The company behind that product, Atlassian, has other products. One of them is called Bitbucket, and, similar to Github, it is a Source Code management system. My personal opinion is that Bitbucket is cheaper, but also that Github is better.

Anyway, in our company we have an old version of Bitbucket hosted on premises. And, even if we take backups and test them, we’ve considered moving to the cloud version. Since we’re using Jira and Confluence (another product, sort of a wiki) in the cloud, moving the SCM seems logical.

### When there is a need, there is a will

However the migration for Bitbucket from on premises to the cloud is not a trivial matter. As far as I know, [there is no tool from Atlassian](https://confluence.atlassian.com/confeval/other-atlassian-evaluator-resources/migrating-from-server-to-cloud).

![](/img/1*YTtErALn00hesc_ISWnBHg.jpeg)Old screenshotThe recommendation for that process is to just import the code. And if you want to do something fancier than moving the code you’re told to [build a product that uses Bitbucket APIs](https://community.atlassian.com/t5/Bitbucket-questions/Migrating-from-Stash-to-Bitbucket/qaq-p/57405).

I’m really surprised about Atlassian not having a tool. And with a considerable number of repositories, even moving the code was not a trivial matter.

So I’m in the process of creating [a tool to help migrating git repositories](https://gitremote.site). The plan is to use it for my company but also at some point to even sell it through the Internet.

### Bitbucket API

But in this post, I just wanted to talk about APIs.

The documentation for the Bitbucket API is [reasonably detailed](https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Busername%7D/%7Brepo_slug%7D/pullrequests). However, it seems generated with some kind of tool in a format that I couldn’t understand what was required, what was optional, examples…

To the point I was doing something like this

curl -X POST https://<usr>:<pwd>[@api](http://twitter.com/api "Twitter profile for @api").bitbucket.org/2.0/repositories/<namespace>/<project>/pullrequests -d '{
"title": "test",
"state": "OPEN",
"source": {
"type": "repository",
"name": "<project>",
"full_name": "<namespace>/<project>",
"scm": "git",
"branch": {
"name": "<branch_name>"
}
},
"is_private": true,
"destination": {
"type": "repository",
"name": "3pageapp",
"full_name": "<namespace>/<project>",
"scm": "git",
"branch": {
"name": "<branch_name>"
}
}
}'and I was getting the following error

{“type”: “error”, “error”: {“fields”: {“source”: [“This field is required.”], “title”: [“This field is required.”]}, “message”: “Bad request”}}How was it possible? I was sending the source field and the title field, but it was as if it wasn’t being received.

It is very very difficult to debug an API only by invocations. I dedicated a few hours of trying to debug the issue, with different combinations of parameters.

In the end, the issue was much more simpler.

You need to send a header to indicate it is a json request.

curl <...> -H "Content-Type: application/json"Probably others will need less time debugging the issue. IMO Bitbucket should say “you need to send json content-type”.

But in any case, if you arrive from Google, now you know it.
