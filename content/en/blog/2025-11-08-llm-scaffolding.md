---
date: 2025-10-08T18:30:00
title: LLM-scaffolding
subtitle: AI interfaces beyond the chat
tags:
  - Technology
categories: [Technology]
toc: false
---

## LLM-scaffolding

A couple of weeks ago I went to an event about AI and trading. The usual "oh yes, AI is great, it is going to change everything". But almost every speaker said something along the lines "but it won't be a chat app".

Yeah, chats are great. Everybody uses them.

But I very much doubt that 5 years from now, the way we interact with AI will be a chatbox.

But if not chat, what?

In this post I'm going to try to present a possible answer. A way of developing rich UIs that are AI centric.

I tried to give a shorter version of this answer to someone who is technology savyy (hi Paul) and failed. I also tried to give an even shorter version of the answer to my wife, who is less savyy but has a lot of common sense. I failed as well.

So let's try with a bit of a longer answer. This is a slightly meta post.

### On the shoulders of giants

First I want to express this idea not something I came up myself with. I haven't seen the code for any demo of what I'm presenting here today, but I can point to a few things that certainly contain the core.

In my mind, I blame @javilop from Twitter/X for [opening my mind]. But the previous link is in Spanish, so I asked Grok to create [a translation](https://x.com/i/grok/share/sDTJc64bZrjxVDWSqbL5hsUXV). Pay attention to the section "All software will run on the LLM layer". The other sections are very interesting as well, but that section is relevant for today.

That is the stage. "All software will run on the LLM layer"

The next reference I want to point at is [this other study from Google where they were able to run a model that generates images from Doom](https://gamengen.github.io/), and that the images change in response to the user actions. So yes, people playing Doom but the game Doom is "not coded" anywhere. It is not that the model generates the code for the game Doom. The code is nowhere. The model generates images!!!

The final reference is "Imagine with Claude"

{{< youtube dGiqrsv530Y >}}

I cannot emphasize how important this less-than-two-minutes clip is. If you haven't yet watched it, do it. Do it now. I won't be offended if you never return to this post (I will never know)

### Meet llm-scaffolding-calculator

When I saw that video from Claude, I started thinking about it.

In the end I [resorted to Claude](https://claude.ai/share/415efcec-857a-4c99-a3d4-42c9565f8fa6). Which provided **a possible way**. Is it the same way that "Imagine with Claude" was built? Maybe, maybe not. But I had to test it.

And the test can be found as an open source project.

https://github.com/gonfva/LLM-Scaffolding-Calculator/

You can check it out and play with it. It will require an ANTHROPIC API key, and in my tests it consumed a non-trivial amount of tokens (1$ spend in a couple of days with very simple tests). Plugging in an API key in a random project might not be the best idea.

But you can inspect the code and try to understand it. There is no real need to execute it.


### How does it work?

It is a FastAPI python app, serving a websocket endpoint and a couple of static files

On the websocket endpoint we have a Claude Agent with some tools defined (the tools are the key, so I will explain them better). Every interaction will be sent to the Claude API. The Claude API might invoke as part of its response "use this tool".

When a tool is used, it will change an internal state (that resides on the webserver). Then the loop on the websocket endpoint will send the new state to the frontend, that will change the AI accordingly.

Copying a diagram that Claude code produced ...

```
┌─────────────────────────────────────────────────┐
│ LLM (Architect)                                 │
│ - Understands user intent                       │
│ - Designs UI structure                          │
│ - Defines interaction rules                     │
└────────────┬────────────────────────────────────┘
             │
             │ Tool Calls (JSON)
             ▼
┌─────────────────────────────────────────────────┐
│ Backend (Mediator)                              │
│ - Validates tool calls                          │
│ - Manages session state                         │
│ - Routes significant events back to LLM         │
└────────────┬────────────────────────────────────┘
             │
             │ Rendering Instructions
             ▼
┌────────────────────────────────────────────────────────────────────────────────────┐
│ Client (Executor)                                                                  │
│ - Renders UI from tool calls                                                       │
│ - Maybe executes behaviors locally (fast loop, not initially, optimization later)  │
│ - Notifies backend only for significant events                                     │
└────────────────────────────────────────────────────────────────────────────────────┘
```

### The primitives

The key is to correctly define those tools that will be used by the Claude API. In Claude's words


#### Defining Primitives is the Main Difficulty

This is the hardest part of the whole system. You need to find the right abstraction level:

+ Too low-level:
```
- create_div(id, class)
- set_style(element_id, property, value)
- add_event_listener(element_id, event, handler_id)
```
+ Too high-level:
```
- create_complete_calculator()
- create_complete_text_editor()
```
+ Just right (probably):
```
- create_window(title, width, height) → window_id
- add_text(window_id, content, style?)
- add_input(window_id, type, placeholder) → input_id
- add_button(window_id, label) → button_id
- on_button_click(button_id) → trigger_event
- get_input_value(input_id) → value
- update_text(element_id, new_content)
```

The difficulty is:
- **Granularity**: Finding the right level of abstraction
- **Event handling**: Deciding which interactions loop back to Claude vs. handled client-side
- **State management**: What state lives where (browser vs. backend vs. Claude's context)

### Why the name LLM-scaffolding?

Because Claude decided it

```
My Personal Favorite:
"LLM-Scaffolded Interaction" or "Declarative Agent UI"
These capture the essence: the LLM sets up the structure declaratively (via tools),
but doesn't participate in the runtime execution loop.
```

### So slow, man

If you happened to download my code and tried to run it you will see that it is slowly. Slow to load the calculator. Slow to react to the key press.

Who in his/her right mind would use something like this? Something so slow for a simple calculator!!!

The code is slow because I really wanted to show how the paradigm works. "Can you optimize it?" Yes. When I was discussing with Claude how to approach this LLM-scaffolding thing, it pointed that the best approach would be to have something hybrid (some interactions on the browser, some others on the LLM). Check the conversation on the link above.

So, yes I could speed up a lot just adding some callbacks.

And the backend server execution could make use of some async combined with real queues. Maybe even a different programming language.

Do I need to send the whole state to the browser? Or only the deltas?

Can I have create_calculator primitive?

The number of optimizations are significant.


But we would all miss the point.

### But why?

"OK. Let's assume speed doesn't matter. Why would you run a calculator with an LLM layer"

Ah, my friend. That's the key question.

And the answer is
    __what if the app you use all day could anticipate to your desires?__
    __What if the UI was able to surface relevant information **to you**.__
    __Present in a way that matters **to you**.__

No matter what kind of app. Be it JIRA, email, or trading software.

Chat won't be the future UI. And ["Her"](https://en.wikipedia.org/wiki/Her_(2013_film)) is an interesting movie, but I don't think it will be audio only either.

### Conclusion

I wanted to understand "Imagine with Claude". I asked Claude. And I explored this LLM-scaffolding thing.

I don't I don't even know if this was the way "Imagine with Claude" was coded. There might be a different way.

But what I do see is that these interfaces might be coming.

And I'm not the only one

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Elon Musk tells Joe Rogan about what&#39;s to be expected with phones.<br><br> <a href="https://t.co/F8bCE2Tkac">pic.twitter.com/F8bCE2Tkac</a></p>&mdash; Joe Rogan Podcast News (@joeroganhq) <a href="https://twitter.com/joeroganhq/status/1984486335835414972?ref_src=twsrc%5Etfw">November 1, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
