---
date: 2025-09-14T18:30:00
title: Audio and emails
subtitle: Agent coding. This is the way
tags:
  - Technology
categories: [Technology]
toc: false
---

## Email Whispery

After [Xowit](https://xowit.com) and [Kids Xowit](https://kids.xowit.com), the third experiment of the year is up.

It's called [Email Whispery](https://email.whispery.site/).

You give temporary access to your email account (yes, I know it is a big ask). The app fetches up to 20 emails from the last 24 hours, and it prepares an audio summary of those messages.

![](/img/result-email-whispery.png)

I'm really surprised that **the audio is so good**. You can [hear a test here](https://gonfva.f-v.es/summary-email.mp3).


I'm not even using the state-of-the-art models. I'm using Chirp 3, which is failry recent, but there are other better models. You pass a text to it, and it puts breaks, entonation... It even understands that whispery.site is pronounced whispery dot site and the dot is not the end of a sentence.



### Embarrassment

When I launched Xowit I quoted the famous sentence

 ["If you're not embarrassed by the first version of your product, you've launched too late"]({{< ref "2025-03-24-xowit.md" >}}).

Well, I'm embarrassed with Email Whispery.

Deeply embarrassed.

The app needs to request OAuth access to Google and permission to read Gmail. Since reading email is a protected activity, I need to go through Google's verification process, which includes recording a video. The outcome is uncertain. I could wait for the verification to finish (I haven't even recorded the video), but I would need to add one by one the emails of people testing

Instead, people will get a very scary screen telling them "go away" and they need to click through an "Advanced" link

![](/img/Screenshot_2025-09-14_104329.png)

Oh, boy. That's bad. Yes, real bad.

I will create a video, and at least that can show the process.

At the same time I doubt that anybody will test the system. Yes, permissions are read-only and immediately revoked. The only thing that persist is the audio and only for one day.

But control of personal email is scary. In my case, I might have risked testing with one of those secondary Gmail accounts that some of us have. And only with someone I trust.

But I didn't want to wait.

### Why

"If you're not planning to do it perfect, why doing it at all?"

Several reasons, including a big one with its own section later on.

Yes, [I want to keep experimenting]({{< ref "2025-03-24-xowit.md" >}}).

Yes, [I want to play more with audio]({{< ref "2025-06-21-kids-xowit.md" >}}).

Yes, **email is like COBOL**. Suposedly extinct but in reality still alive and healthy.

I've got plenty of ideas around email. And although they are not valid for a side project with zero funding, this idea had been in my mind for a while and it shouldn't be too expensive.

Besides, ["Her"](https://en.wikipedia.org/wiki/Her_(2013_film) is an interesting movie.

But the real reason is a controversial thought.

### Controversial vision

Vibe coding, or agent coding, or whatever you want to call it **is the future**.

You will program in English.

Yes. Some people will still work directly with Javascript, or Python, or Rust, or Java, or C++, or Ruby. Like some people still do work with assembly.

But for many people, the programming language will be English.

"No way, Gonzalo. Algorithms are better than relying on the random whim of an LLM"
"Haven't you read all those stories of people vibe coding, launching and being hacked? Or asking for a failing test and LLM deleting the whole suit? Haven't you heard about ["context rot"](https://x.com/simonw/status/1935478180443472340)"

Yes. I feel your pain. I've been there.

I've felt the anger at Claude code. The "WTF are you effing doing" thought. The "Don't 'you're absolutely right' bullshit me and listen to what I'm telling you" thought. My polite "That didn't work either. Please think harder and plan before doing any changes" when you wanted to shout "you effing moron don't have an effing clue. Don't effing change an effing letter of the code until you're effing sure of what you're effing doing".

Yes, the "you've reached your 5h limit".

Yes. I discovered that GOOGLE_CLIENT_ID was being leaked to client code (and likely other sensitive variables).

The rage was too much to put into words. But I've felt it.

**I've felt all of that**.

At the same time, I feel this is the way we will program. And I better learn how to do it.

![](/img/this-is-the-way.png)

And I'm learning.

And again, for me the best model is to talking to a technically-great-but-very-junior engineer. You have to be very detailed. You have to give it a detailed plan. You have to verify what it gives back. And of course, don't take it personal.

I've launched three (toy) apps this year in a few weekends. I'm creating a (toy) app at work to help with FinOps.

I'm learning.

This app is probably the last one with a "vibe" approach. The next one will more agentic. More purposeful. With more automated test. Deployments from day one. With task definitions as Github issues. Tools defined. Workflows defined.

And by the way, I used a third-level domain (email.whispery.site) because I have other plans for the second level (whispery.site)
