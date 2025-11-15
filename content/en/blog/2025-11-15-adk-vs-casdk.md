---
date: 2025-11-15T18:30:00
title: Some musings on agents
subtitle: Agent Development Kit vs Claude Agent SDK
tags:
  - Technology
  - Agents
categories: [Technology]
toc: false
---

## Some musings on agents

This week I followed an interesting [course on AI agents](https://www.kaggle.com/learn-guide/5-day-agents).

Many of you might have heard about agents. AI Agents are an evolution of the initial language models that did just a discrete task (translate, answer a question...).

They have a (limited) ability to reason, and act. Citing one of the whitepapers, they usually incorporate the language model ("the brain"), some tools ("the hands"), and the orchestration layer ("the nervous system").

I had heard about agents. But I hadn't look into them, and this course has been eye-opening.

To the point that I recognised some of the patterns (and gotchas) I had been following (and finding).

For example, [Kids-Xowit](https://kids.xowit.com) the [orchestration called](https://gonzalo.f-v.es/blog/2025-06-21-kids-xowit/#safety) one LLM to verify a prompt, then another LLM to generate a story, then another LLM to verify the story, then TTS to generate audio. But these calls were explicit, in code.

### The hammer and the nail and all that...

Once I discovered Google's Agent Development Kit (ADK), that workflow mapped to a linear workflow agent with subagents. For the Kids-Xowit, I could define [Sequential Agent](https://google.github.io/adk-docs/agents/workflow-agents/sequential-agents/) that invokes:

+ Subagent 1 ([LlmAgent](https://google.github.io/adk-docs/agents/llm-agents/) with a verifier model): Takes user prompt, outputs if it is a valid prompt.
+ Subagent 2 (another LlmAgent): Generates the story using the verified prompt.
+ Subagent 3 (verifier again): Scores/refines the story for age-appropriateness.
+ Subagent 4 (tool-integrated agent): Calls Google TTS for audio, treating it as a custom function tool.

### Claude Agent SDK (CASDK) vs Google's Agent Development Kit (ADK)

This came a bit of a surprise. I had thought CASDK was more advanced (certainly Claude Code, which is based on the SDK seems powerful). However, it _felt_ ADK is significantly more powerful in its primitives.

Whether or not the underlying models, or others are better, is a completely different topic. It seems you can use ADK with for example [LiteLLM](https://www.litellm.ai/).

In general CASDK seemed more like "trust it will work" and ADK is more explicit. That's obviously assuming you don't want explicit filesystem tools (i.e. we're talking about web-applications). The automatic context compaction in CASDK is relevant. But apart from that, CASDK seemed incredible poor, compared with the Lego-approach of ADK.

"What am I missing?"

### Level 4

To give a bit of context, one of the whitepapers in the course classes agents in 5 levels:
+ Level 0 is the original core reasoning. Models could only reply based on what they "know" from "inside".
+ Level 1 (connected problem solver) is more advanced. It can get information from outside.
+ Level 2 (strategic problem solver) is even more advanced and can plan multiple steps.
+ Level 3 (collaborative multi-agent) moves from a single super-agent to a team of specialists.
+ Level 4 (self evolving system). When the agent detects something that it cannot do, it builds an agent, and then it invokes the agent.

With that context, I was thinking about some project (Hi Phil), discussing with Claude Code. I came to something that was worth sharing. I asked, "I'm going to share this. Could you generate a PDF on it".

I thought that maybe it was goint to use a common tool or something.

Instead, Claude Code (which is now an agent itself), detected it didn't have access to such a tool, so proceeded to generate the Python code for a (narrow) tool that generates the PDF, and then it invoked the tool.

And it did an amazing job (apart from the links at the end)

If you don't trust me, please, see the [CC chat](https://claude.ai/share/10f4e69b-8c78-40cf-a1aa-44563123416b)

So Claude Code (and I assume Claude Agent SDK) can build its own agents!!!

<div class="tenor-gif-embed" data-postid="17552382" data-share-method="host" data-aspect-ratio="1.52381" data-width="100%"><a href="https://tenor.com/view/blow-mind-mind-blown-explode-gif-17552382">Blow Mind Mind Blown GIF</a>from <a href="https://tenor.com/search/blow+mind-gifs">Blow Mind GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>

Maybe Claude Agent SDK is not so poor after all!!.
