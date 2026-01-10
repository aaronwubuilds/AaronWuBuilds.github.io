---
title: "Backing Up My Brain Before the Army: How I Automated My Blog Workflow with Python & AI"
date: 2026-01-10T12:00:00+08:00
slug: blog-automation-workflow-en
tags: ["Automation", "Python", "Hugo", "AI Engineering", "System Design", "Build in Public"]
canonical_url: "https://aaronwubuilds.github.io/posts/blog-automation-workflow-en/"
description: "This isn't an 'AI is magic' hype post. It's a realistic look at how I acted as the Architect to an AI Executor, fighting hallucinations and bugs to build a content pipeline before my military service."
---

中文版請點此 🇹🇼 👉 [入伍前把腦袋備份：我如何用 Python 和 AI 自動化我的技術部落格](https://aaronwubuilds.github.io/posts/blog-automation-workflow-zh/)

---

**Status:** ☕️ Caffeinated | ⏳ Days until Enlistment: 20

I'm Aaron, a backend engineer who loves Software Design.

Imagine this scenario: You're a complete novice. You have no idea what creates value, or who your audience is. You don't even know where to publish your insights—even though the tech you're working on is interesting and worth documenting. What do you do?

Deep research? Well, that was my initial thought. But a mentor running a one-person company told me that the most important thing when targeting a market is to **Fail Fast**. You need to test rapidly, throwing content onto various platforms to see the reaction.

I initially pushed back. "I'm not selling a product, why keep talking about 'the market'?" But after studying numerous startup successes and failures, I found a pattern: the most tragic endings usually come from "overthinking until the runway burns out" and "hallucinating what the market looks like."

So, I decided to start posting everywhere. But day one was frustrating: GitHub Pages, Dev.to, X, Threads, LinkedIn... every platform has different formatting. I spent an entire afternoon and evening just handling the Canonical URL and OG Image for a single intro post about "How Redis Solves Race Conditions."

**I was furious.**

To make matters worse: **I'm leaving for mandatory military service in 20 days.**
To ensure my blog doesn't die while I'm in the barracks, I decided to "automate" the entire publishing process. I definitely don't want to spend my precious leave time copy-pasting articles.

### 🛠️ The Architecture: Brain First, AI Second

There are plenty of tutorials online on how to stitch together pipelines with various tools, but I didn't copy them.

As an engineer, I'm used to **defining the problem myself**. Before opening the AI chat window, I already had a complete architectural blueprint in my head:

1.  **Input:** I only provide a raw idea (in Chinese) and keywords.
2.  **Process:** The system must automatically expand this into a full article, translate it into English, and generate social media posts tailored to each platform (Professional for LinkedIn, casual for Threads).
3.  **Output:** Produce Hugo-compliant Markdown files, where the filename and Front Matter must match my specs precisely.

I made the requirements clear: I didn't want the AI to "think" of a workflow for me; I wanted the AI to "write" the **System Prompt** that executes this workflow.

It sounds simple, but during implementation, I realized that **AI is like a fresh Junior Engineer: very smart, but it loves to be a smart-ass.**

### 🐛 Debugging the AI: When the Machine Tries to be Creative

The birth of this system was essentially a **Code Review war between a Senior Engineer (Me) and the AI.**

#### 1. The War on Naming Conventions

My brand name is **AaronWuBuilds**. But when generating Config and SSH settings, the AI constantly tried to simplify it to `AaronBuilds`.
Does that sound trivial? Wrong. In an SSH Config, a single character difference causes `Host verification failed`, blocking my Git Push entirely.
I had to act like a strict Tech Lead, repeatedly correcting it: "Do not change my Variable Names! Write exactly what I said!"

#### 2. The Attack of Feature Creep

During initial testing, my site build failed. After investigating, I found the AI had arbitrarily inserted a Shortcode into the generated Markdown.
It seemed to think every blog *must* have a newsletter subscription box, but my current Hugo Theme doesn't even have that feature configured!
I had explicitly said "Keep It Simple," yet I had to issue a direct order: "Remove all Shortcodes. I do not need this feature right now."

If I hadn't corrected this immediately, this hidden bug would have certainly exploded during a future automated deployment—likely when I'm stuck in the army camp, unable to put out fires.

This taught me a crucial lesson: **AI can generate all the data, but the "Spec" and "QA" must remain in human hands.**

---

### ⚙️ The Solution: Aaron Wu Builds Pipeline v1.0

After fixing all the "smart-ass" errors mentioned above, here is the high-speed publishing system I use now:

#### **Step 1: The Input**
I simply prepare a **Chinese Outline (Raw Idea)**, today's date, and a **Slug**.

#### **Step 2: The Compiler (Prompt Engineering)**
I feed the Input into my fine-tuned Prompt. Because of the previous strict corrections, it now obediently spits out:
1.  **Chinese Markdown** (with correct Cross-links).
2.  **English Markdown** (with Canonical URL for SEO).
3.  **Social Media Pack** (English for LinkedIn, Chinese for Threads, links pre-filled).
4.  **Image Directives** (Prompts for DALL-E/Midjourney).

#### **Step 3: The Build & Deploy (Git Operations)**
I save the two Markdown files into the project and execute:

~~~bash
git add .
git commit -m "New Post: Blog Automation Workflow"
git push
~~~

At this point, GitHub Actions automatically builds the static site and pushes it live.

#### **Step 4: The Distribution (Sync)**
Dev.to automatically fetches my English articles via RSS (a specific SEO strategy we set up), so I just need to log in and click "Publish."

---

### 🎯 The Takeaway

This workflow compressed a process that used to take 4-5 hours (Writing + Translating + Formatting + Publishing) into less than **30 minutes**.

This gave me two major insights:

1.  **Don't let AI handle the Architecture:** The architecture comes from the human mind. If you don't know what you want, AI will just give you a pile of unrunnable garbage code. You must have a clear Pipeline design first for AI to be a powerful implementation assistant.
2.  **Review Everything:** AI hallucinates, gets lazy, and changes variables on a whim. As an engineer, the ability to **Code Review** is now more important than the ability to write code.

20 days until enlistment.
For every day remaining, I will use this system—designed by me, executed by AI—to back up all my CS knowledge from Rutgers, the enterprise architecture I saw at AMD, and my pre-enlistment survival anxiety.

Stay tuned.

***

*If you enjoyed this post, follow me for more insights on Backend Engineering and real-world automation.*