---
title: "Prompts as a Code"
date: "2025-08-06"
draft: false
tags: ["ai","llm","backups","blog"]
---

After spending hundreds of hours working with different LLM applications - IDEs (VSCode, Windsurf, Cursor, Cline, Cody, ClaudeCode, Codex, Copilot), chats (ChatGPT, Claude, OpenWebUI, and a bunch of my own), and flow designers (n8n, make, rapier, flowise, power automate, copilot studio) - I've learned a few things about what really happens when you move beyond simple chat interactions with LLMs. I've also built some opinions. 

## What Are Prompts, Really?

A prompt is a short instruction, usually written in natural language, that tells a language model what kind of output you want. Prompts can be used to ask questions, write summaries, generate code, or do many other tasks.

In many LLM applications, prompts are sent together with other types of messages - for example, system messages that set the model’s tone or behavior. Some systems also add extra context, like previous chat history or documents, to help the model give better answers.

The most important thing is to make your prompt clear and specific. In most cases, the more detail you give, the more useful and accurate the response will be.

## Why Backing Up Prompts and Is It Actually Smart?

Backing up prompts sounds like a good idea - and versioning them too. Some prompts can be reused often: text generation for articles, emails, responses to customers, report generation, etc. They can be improved or adjusted over time. And sometimes you'll learn that your latest prompt makes the LLM produce output you like less than the previous version. You want to go back.

If you're working with chat interfaces, this comes built-in. These things have databases that store your chats - prompts and responses. The UI is ready for this: you can browse, search, copy, export. Some UIs even let you choose a prompt template to start with (looking at you, Copilot). Typically, you find the previous chat, copy and paste, adjust as needed. Everything stays in the database.

## The Evolution: From Chat to Automation

But here's where it gets interesting. If you find yourself working with the same chats, the same prompts very often, you start thinking about automation. So you stop using the chat interface and start using the LLM's API. 

Now you need code to call the API - Python, Java, JS, Go, whatever. The code has to handle sending prompts, creating message objects for the LLM's API, handling responses, and returning them to the calling system. You start thinking about tools that do this for you: Make, n8n, Flowise, Dify. Great! You have a visual, block-based flow built with no-code tools. Your flow ends up as a YAML/JSON file or sitting in a database.

But what if you don't want to use these tools? You can do Python. Great - this is code. And the prompt becomes part of your code. Code goes into source versioning solutions like Git (there are others, but Git is the standard nowadays). Git becomes your backup and lets you go back to any checked-in version from the past, showing differences between versions.

## The Evaluation Problem: LLMs Are Not Deterministic

Here's the thing that trips up most people: to know if your prompt is good, you have to run it and evaluate the results. The problem? LLMs (as of today) are NON-DETERMINISTIC.

I tested this with three identical one-word prompts. Three different results. The more words, the more randomness gets introduced. Most LLMs use non-deterministic decoding methods like top-k sampling, top-p (nucleus) sampling, and temperature scaling. These introduce randomness to encourage varied, natural-sounding language. Unless you explicitly set a seed and use greedy decoding (e.g., temperature=0, top-k=1), the model will likely generate different outputs on repeated runs.

What if you actually use greedy decoding?

**Default Decoding (temperature ~0.7, top-p=0.9):**
- Prompt: "What is the capital of France?"
- Answer: "The capital of France is Paris. It's one of the most iconic and visited cities in the world."

**Greedy Decoding (temperature=0, top-k=1):**
- Prompt: "What is the capital of France?"
- Answer: "Paris."

For coding, this means we're disabling all the possible optimizations that higher "temperature" might bring, but greedy decoding will miss creative solutions because it goes with the most "seen" pattern in training data.

Why do I mention this? The quality of output might be perceived differently depending on the use case. And it can be different every time, even if the input is exactly the same. It needs visual inspection and human analysis to decide if it's good or bad.

If the output needs to be in a very specific format, reproducible, and run many times by different people - well, you probably guessed it. You create a script. You ask the LLM to create a script. Code. SQL, Python. Something that will produce the same output every single time, so you can actually measure it.

## The Tools Landscape Evolution

I've watched the LLM tools landscape evolve rapidly. If there's a gap somewhere, it lands in these tools instantly. People learned that LLMs work better if you ask them to work in steps or create a plan. Within a month, all major tools supported "plan mode" (where the LLM breaks big tasks into small ones and reports back on progress and current focus). Same with prompt templates, coding instructions per project, etc.

## "Engineering" for Non-Engineers?

I recently saw someone suggest we should "borrow the best parts of engineering, but without asking everyone to become an engineer" - treating domain knowledge like code and "writing" domain-driven instructions.

Oh. WOW. Why has nobody ever thought about that? 😉 

Remember FoxPro, Informix 4GL, PowerBuilder? IBM Rational Rose, Oracle Designer? Oracle Express/APEX (Application Express)? 

I think at the end of the day, engineers are engineers not because they know the tools. It's because they think in a way that allows them to understand how to use tools to solve problems. (There will be a small population with overlap - like SuperRyan = Engineer+Sales)

## The Agent Workflow Reality Check

Finally, about agents running for 3+ hours. Agents can rely exclusively on LLM thinking (transformer-based) or implement different frameworks (tree of thoughts, graph of thoughts, etc.). In any case, in a multi-step process, you'll have dozens or hundreds of changes per hour. 

How is backup going to help you go back to the point where it was okay? If you're just trying to drop all changes from the last 3 hours, what's the challenge? You just take the last correct Git commit. If you want to save some work, you need to review it - and I don't see people reverting from 50 backup snapshots. I see them using some UI tool built on top of Git (or the next generation of Git).

This is totally out of our area of expertise. Guess what? Eventually, it all lands in some database or PVC running on Kubernetes. So where should our focus go? 😉

## The Bottom Line

Working with LLMs professionally means understanding this evolution from simple chat to complex workflows. It means grappling with non-deterministic outputs and building systems that can handle that reality. And it means recognizing that while the tools evolve rapidly, the fundamental engineering thinking patterns remain constant.

The real challenge isn't backing up prompts - it's building robust systems that can work with the inherent unpredictability of these powerful but inconsistent tools.