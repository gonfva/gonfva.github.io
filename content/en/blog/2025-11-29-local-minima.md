---
date: 2025-11-29T12:30:00
title: Local minima?
subtitle: Scaling wall or just different paths
tags:
  - Technology
  - Agents
categories: [Technology]
toc: false
draft: true
---

## Local minima?

This is one of those posts you want to write, because writing is the best way to sort out one's thoughts.

But as I start typing this, I don't really know what I want to say.

I know it relates to my most recently experiences programming with agents.

And I know it also relates to what is called the scaling wall.

And I feel that even if I knew that AI will affect everything, even to the point of me being unemployed, I felt AI would be net possitive for the society. I was never on the "Shoggoth" camp.

I haven't changed camps, but I have started feeling more concerned.

### An experience with Gemini 3

You might have seen I just launched a new experiment about video-communication. The idea about the app had been on my mind for a few months. But its current incarnation (including group calls, translations to Spanish and English and deployment) has not required more than 2-3 days of work.

And it started playing with Gemini 3 pro, using antigravity. And I started with an epic prompt. Yes, I know that's not how you do it. I know you have to specify one feature at a time. I usually do that.

![](/img/2025-11-29-Initial prompt.png)

But I wanted to know what Gemini 3 was capable of. And what it did blew my mind. Because it proceeded to cook a very nice first version of both frontend and backend.

But then I started noticing things. And I'm going to put just one example.

When I am reviewing LLMs work, sometimes I notice two things, and I mention them on the same prompt. Again I shouldn't do this, but it is also a way to evaluate. In this case, while debugging a deployment, I noticed that on line 55 it was copying a file `.env.example` that the LLM had created (not the actual `.env` that I used for the secrets). So I mentioned as a second part of the prompt

![](/img/2025-11-29-Line55-1.png)

It didn't do anything. So I mentioned a second time, on a separate prompt

![](/img/2025-11-29-Line55-2.png)

It still proceed to miss completely the mark. So I had to repeat it a third time

![](/img/2025-11-29-Line55-3.png)

This was not an isolated issue. I noticed things that showed that it wasn't quite following what I was asking. Things that showed an amazing over-confidence.

I ended up abandoning Anti-gravity and Gemini 3, and reverted back to my usual Claude Code.

### An "isolated incident" with Claude





https://www.youtube.com/watch?v=QxYmm5yCJBg

Monkey with guns
