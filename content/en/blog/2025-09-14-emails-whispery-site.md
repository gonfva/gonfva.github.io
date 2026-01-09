---
date: 2025-09-14T18:30:00
title: Audio, emails, and agent coding
subtitle: This is the way. And I'd better learn how to do it
tags:
  - Technology
  - Agents
  - Experiments
categories: [Technology]
toc: false
featured: true
---

## Email Whispery

After [Xowit](https://xowit.com) and [Kids Xowit](https://kids.xowit.com), the third experiment of the year is up.

It's called [Email Whispery](https://email.whispery.site/).

It produces brief email summaries in an audio format.

You give temporary access to your email account (yes, I know it is a big ask, and I wouldn't use such an app). The app fetches up to 20 emails from the last 24 hours, and it prepares an audio summary of those messages.

![](/img/result-email-whispery.png)

I'm really surprised that **the audio is so good**. You can [listen to an example here](https://media-gonzalo.f-v.es/summary-example-email-whisery.mp3).


You pass a text to it, and it puts breaks, intonation...

I'm not even using the state-of-the-art models. I'm using Chirp 3, which is fairly recent. But there are even better models.

### Embarrassment

When I launched Xowit I quoted the famous sentence

 ["If you're not embarrassed by the first version of your product, you've launched too late"]({{< ref "2025-03-24-xowit.md" >}}).

Well, I'm embarrassed with Email Whispery.

Deeply embarrassed.

The app needs to request OAuth access to Google and permission to read Gmail. Since reading email is a protected activity, I need to go through Google's verification process, which includes recording a video. The outcome is uncertain. I could wait for the verification to finish (I haven't even recorded the video), and remain in testing. But I would need to add the emails of testing users one by one.

Instead, I moved to production, so people will get a very scary screen telling them "go away" and they need to click through an "Advanced" link

![](/img/Screenshot_2025-09-14_104329.png)

Oh, boy. That's bad. Yes, real bad.

I will create a video for Google verification. And I can reuse the video, to show what the app is for.

However, I doubt that anybody will test the system. Yes, permissions are read-only and immediately revoked. The only thing that persist is the audio and only for one day.

But **control of personal email is scary**. In my case, I might have risked testing with one of those secondary Gmail accounts that some of us have. And only if the app came from someone I trust.

So I didn't want to wait just in the hope of people using it.

### Why

"If you're not planning to do it perfect, why do it at all?"

Several reasons, including a big one with its own section later on.

+ [I want to keep experimenting]({{< ref "2025-03-24-xowit.md" >}}).
+ [I also want to play more with audio]({{< ref "2025-06-21-kids-xowit.md" >}}).
+ **email is like COBOL**. Supposedly extinct but in reality still alive and healthy. I've got plenty of ideas around email. And although they are not valid for a side project with zero funding, this idea had been in my mind for a while and it shouldn't be too expensive.
+ ["Her"](https://en.wikipedia.org/wiki/Her_(2013_film)) is an interesting movie.

But the real reason is controversial.

### Controversial vision

Vibe coding, or agent coding, or whatever you want to call it **is the future**.

![](/img/main-standing-on-the-court.png)

You will program in English.

Yes. Some people will still work directly with Javascript, or Python, or Rust, or Java, or C++, or Ruby. Like some people still do work with assembly.

But for many people, the programming language will be English.

> _"No way, Gonzalo. Algorithms are better than relying on the random whim of an LLM"._

> _"Haven't you read all those stories of people vibe coding, launching and being hacked? Or asking for a failing test and LLM deleting the whole suite? Haven't you heard about ["context rot"](https://x.com/simonw/status/1935478180443472340)"_

Yes. I feel your pain. I've been there.

I've felt the anger at Claude code.

The _"WTF are you doing"_ thought. The _"Don't 'you're absolutely right' BS me and listen to what I'm telling you"_ thought. My polite _"That didn't work either. Please think harder and plan before doing any changes"_ when you wanted to shout _"Don't effing change an effing letter of the code until you're effing sure of what you're effing doing"_.

Yes, the _"you've reached your 5h limit"_.

Yes. I discovered that GOOGLE_CLIENT_ID (and other variables) was almost being **leaked** to client code.

**I've felt all of that**.

At the same time, I feel this is the way we will program. And I'd better learn how to do it.

![](/img/this-is-the-way.png)

### And I'm learning

Slowly learning.

And as I mentioned on a different post, for me the best mental model is **talking to a technically-great-but-very-junior engineer**. You have to be very detailed. You have to agree on a very specific plan. You have to verify what it gives back. And of course, don't take it personal.

I've launched three (toy) apps this year in a few weekends. I'm creating a (toy) app at work to help with FinOps.

I'm learning.

This app is probably the last one with a "vibe" approach.

The next one will be more agentic. More purposeful. With more automated tests. Deployments from day one. With task definitions as Github issues. Tools defined. Workflows defined.

We'll see how it goes.
