---
date: 2025-11-15T12:30:00
title: Some musings on agents
subtitle: Agent Development Kit vs Claude Agent SDK
tags:
  - Technology
  - Agents
categories: [Technology]
toc: false
---

## Discovering agents

This week I followed an interesting [course on AI agents](https://www.kaggle.com/learn-guide/5-day-agents).

Many of you might have heard about agents. I had heard about agents, but I hadn't looked into them 😳, and this course has been eye-opening.

But for those who (like me a week ago) don't have a clear picture, AI agents are an evolution of the initial language models that performed only discrete tasks (translate, answer a question, etc.).

They have a (limited) ability to reason and act. As one of the whitepapers in the course describes it, they usually incorporate the language model ("the brain"), some tools ("the hands"), and the orchestration layer ("the nervous system").

After the course, I have started recognized some of the patterns (and gotchas) I had been following (and finding).

For example, in [Kids-Xowit](https://kids.xowit.com) I [called](https://gonzalo.f-v.es/blog/2025-06-21-kids-xowit/#safety) one LLM to verify a prompt, then another LLM to generate a story, then another LLM to verify the story, then TTS to generate audio. But these calls were explicit, in code. Not from an agent

### The hammer and the nail and all that...

Once I discovered Google's Agent Development Kit (ADK), that workflow mapped naturally to a linear workflow agent with subagents. So for Kids-Xowit, I could define a [Sequential Agent](https://google.github.io/adk-docs/agents/workflow-agents/sequential-agents/) that invokes:

+ Subagent 1 ([LlmAgent](https://google.github.io/adk-docs/agents/llm-agents/) with a verifier model): Takes user prompt, outputs if it is a valid prompt.
+ Subagent 2 (another LlmAgent): Generates the story using the verified prompt.
+ Subagent 3 (verifier again): Scores/refines the story for age-appropriateness.
+ Subagent 4 (tool-integrated agent): Calls Google TTS for audio, treating it as a custom function tool.

### Claude Agent SDK (CASDK) vs Google's Agent Development Kit (ADK)

This came as a bit of a surprise. I had thought CASDK was more advanced (certainly Claude Code, which is based on the SDK, seems very powerful).

However, ADK felt significantly more powerful in its primitives.

In general, CASDK seemed more like "trust it will work," while ADK is more explicit. That's assuming you don't need explicit filesystem tools (we're talking about web applications). The automatic context compaction in CASDK is relevant. But apart from that, CASDK seemed incredibly poor compared with the Lego approach of ADK.

Whether or not the underlying models are better is a completely different topic. For example, it seems you can use ADK with [LiteLLM](https://www.litellm.ai/). So it is not ADK leads you to Gemini only.

"What am I missing?"

### Level 4?

To give a bit of context, one of the whitepapers in the course classifies agents into 5 levels:
+ Level 0 is the original core reasoning. Models could only reply based on what they "know" internally.
+ Level 1 (connected problem solver) is more advanced. It can get information from outside.
+ Level 2 (strategic problem solver) is even more advanced and can plan multiple steps.
+ Level 3 (collaborative multi-agent) moves from a single super-agent to a team of specialists.
+ Level 4 (self evolving system). When the agent detects something that it cannot do, it builds an agent, and then it invokes the agent.

With that context, I was thinking about some project (hi Phil) and discussing it with Claude Code. I arrived at something worth sharing. I asked, "I'm going to share this. Could you generate a PDF on it"

I thought that maybe it was going to use a common tool or something.

Instead, Claude Code (which is now an agent itself) detected it didn't have access to a general tool. It seems to know there is a skill, but the skill is more like a framework. And following that framework it proceeded to generate the Python code for a (narrow) tool that created the PDF, and then it invoked that tool.

And it did an amazing job (apart from the links at the end).

If you don't trust me, see the [CC chat](https://claude.ai/share/10f4e69b-8c78-40cf-a1aa-44563123416b).

I'm not saying that's level 4. But Claude Code (and, I assume, Claude Agent SDK) can build its own tools!!

<div class="tenor-gif-embed" data-postid="17552382" data-share-method="host" data-aspect-ratio="1.52381" data-width="100%"><a href="https://tenor.com/view/blow-mind-mind-blown-explode-gif-17552382">Blow Mind Mind Blown GIF</a>from <a href="https://tenor.com/search/blow+mind-gifs">Blow Mind GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>

Maybe Claude Agent SDK is not so poor after all!

And maybe I need to rethink my [LLM scaffolding project]({{< ref "2025-11-08-llm-scaffolding">}}).
