Here is the compiled content pack for **Aaron Builds**.

### Part 1: Chinese Markdown File

```markdown
---
title: "[Build in Public] 入伍前把腦袋備份：我如何用 Python 和 AI 自動化我的技術部落格"
date: 2026-01-10T12:00:00+08:00
slug: blog-automation-workflow-zh
tags: ["Automation", "Python", "Hugo", "AI Engineering", "System Design", "Build in Public"]
description: "這不是一篇盲目吹捧 AI 的文章。這是我在入伍前夕，如何以人類作為架構師、AI 作為執行者，克服幻覺與 Bug，構建出自動化發文系統的實戰紀錄。"
---

Read English Version 🇺🇸 👉 [Backing Up My Brain Before the Army: How I Automated My Blog Workflow with Python & AI](https://aaronwubuilds.github.io/posts/blog-automation-workflow-en/)

---

**Status:** ☕️ Caffeinated | ⏳ Days until Enlistment: 20

我是 Aaron，一個喜歡 Software Design 的後端工程師。

想像這個情況：你完全是新手，根本不知道寫什麼東西可以創造價值，甚至不知道 Audience 是誰。你甚至不知道要在哪裡發表你的所學所見——明明這些技術對你而言是有趣且值得紀錄的。你會怎麼做？

深刻的研究嗎？Well，我本來也是這麼想的。不過一位經營一人公司的前輩告訴我，鎖定市場最重要的事情是 **Fail Fast**，也就是要能夠快速地測試，在各個平台都丟出去看看反應。

我一開始很反對，「我又不是要賣東西，幹嘛一直講市場？」不過當我 Study 了好多成功與失敗的創業例子後，發現一個規律：最悲慘的結局，大多來自「想太多，錢和時間就燒完了」以及「我以為市場長這樣」。

於是我打算到處發點東西，但是第一天真的很多挫折：Github Pages, Dev.to, X, Threads, LinkedIn... 每個平台的格式都不太一樣。我光是發第一篇簡介「Redis 如何解決 Race Condition」的文章，處理 Canonical URL 和 OG Image 就花了我一個下午。

**我很生氣啊！**

更尷尬的是：**我再過 20 天就要去當兵了。**
為了確保我的部落格不會因為我進部隊就荒廢，我決定把整個發布流程「自動化」。畢竟我可不想放假回家還把家人丟著，花時間一直在複製貼上文章。

### 🛠️ The Architecture: 先有腦袋，才有 AI

網路上有很多教你用各種工具拼湊 Pipeline 的教學，但我並沒有直接照抄。

身為工程師，我習慣**自己定義問題**。在打開 AI 聊天視窗之前，我腦中已經有了一個完整的架構藍圖：

1.  **Input:** 我只給中文大綱 (Raw Idea) 和關鍵字。
2.  **Process:** 系統必須自動擴寫成完整文章、自動翻譯成英文、自動生成對應各社群平台的貼文（LinkedIn 要專業、Threads 要親民）。
3.  **Output:** 產出符合 Hugo 格式的 Markdown 檔案，且檔案名稱與 Front Matter 必須精準符合我的規範。

我把需求講得很清楚：我不是要 AI 幫我「想」流程，我是要 AI 幫我「寫」出能執行這個流程的 **System Prompt**。

這看似簡單，但在實作過程中，我發現 **AI 就像一個剛畢業的 Junior Engineer：很聰明，但很愛自作聰明。**

### 🐛 Debugging the AI: 那些 AI 試圖搞砸我的時刻

這套系統的誕生過程，其實就是一場**資深工程師 (我) vs. AI 的 Code Review 戰爭**。

#### 1. 命名規範的戰爭 (Naming Convention)

我的品牌名稱是 **AaronWuBuilds**。但 AI 在生成 Config 和 SSH 設定時，總是很雞婆地幫我簡化成 `AaronBuilds`。
這聽起來沒什麼？錯了。在 SSH Config 裡，差一個字就會導致 `Host verification failed`，讓我的 Git Push 直接被擋在門外。
我必須像個嚴格的 Tech Lead，反覆糾正它：「不要亂改我的 Variable Name！照我說的寫！」

#### 2. 殭屍功能的逆襲 (Feature Creep)

一開始測試時，我的網站 Build 失敗了。查了半天，發現 AI 在生成的 Markdown 裡自作主張塞了一個 Shortcode。
它似乎覺得每個部落格都該有個電子報訂閱框，但我目前的 Hugo Theme 根本沒設定這個功能！
我已經明確說了「保持簡單 (Keep It Simple)」，我必須明確下達指令：「移除所有 Shortcode，我現在不需要這個功能。」

如果我不即時糾正，這個隱藏的 Bug 肯定會在未來的某次自動化部署中炸開，到時候人在軍營的我根本救不了火。

這教了我重要的一課：**AI 可以幫你產出所有資料，但「規格 (Spec)」和「品質控管 (QA)」必須掌握在人類手裡。**

---

### ⚙️ The Solution: Aaron Wu Builds Pipeline v1.0

在修正了上述所有「自作聰明」的錯誤後，這就是我現在使用的高速發文系統：

#### **Step 1: The Input (Input Data)**
我只需要準備一個 **中文大綱**、今天的日期、以及一個 **Slug (網址代號)**。

#### **Step 2: The Compiler (Prompt Engineering)**
我把 Input 丟進我調教好的 Prompt。因為之前的嚴格糾正，現在它會乖乖吐出：
1.  **中文版 Markdown** (含正確的 Cross-link)。
2.  **英文版 Markdown** (含 Canonical URL 以利 SEO)。
3.  **社群貼文包** (LinkedIn 用英文、Threads 用中文，連結都自動填好了)。
4.  **圖片指令** (給 DALL-E/Midjourney 用的 Prompt)。

#### **Step 3: The Build & Deploy (Git Operations)**
我將兩個 Markdown 檔案存入專案，執行：

~~~bash
git add .
git commit -m "New Post: Blog Automation Workflow"
git push
~~~

此時，GitHub Actions 會自動建置靜態網站並上線。

#### **Step 4: The Distribution (Sync)**
Dev.to 會自動透過 RSS 抓取我的英文文章（這是我們特別設定好的 SEO 策略），我只要進去按「發布」即可。

---

### 🎯 The Takeaway

這套流程把原本需要 4-5 小時的「寫作+翻譯+排版+發布」過程，壓縮到了 **30 分鐘**以內。

這給了我兩個重要的啟發：

1.  **Don't let AI handle the Architecture:** 架構是人想出來的。如果你連自己要什麼都不知道，AI 給你的只會是一堆無法運行的垃圾代碼。你必須先有清晰的 Pipeline 設計，AI 才能成為最強的實作助手。
2.  **Review Everything:** AI 會幻覺、會偷懶、會自作主張修改變數。身為工程師，**Code Review** 的能力比寫 Code 的能力更重要。

還有 20 天入伍。
接下來的每一天，我都會用這套我自己設計、AI 輔助執行的系統，把我在 Rutgers 雙主修學到的 CS 知識、在 AMD 實習看到的企業級架構、以及身為準役男的生存焦慮，全部備份上來。

Stay tuned.

***

*如果你喜歡這篇文章，歡迎在社群媒體上追蹤我，我會持續分享更多 Backend 技術與自動化實戰經驗。*

```

### Part 2: English Markdown File

```markdown
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

```

### Part 3: Social Media Distribution Pack

```markdown
### 1. Threads / Facebook (Chinese)
**Target:** Taiwan Tech Community / Junior Devs / Founders
**Tone:** Personal, Urgent, "Real talk"

再過 20 天就要入伍了，但我不想讓這個剛開張的技術部落格長草。🌱

很多人說「用 AI 寫部落格很快」，但我實際操作下來發現：如果你沒有一個好的架構，AI 只會給你一堆需要花更多時間 Debug 的垃圾。

為了在當兵期間也能維持 AaronWuBuilds 的運作，我花了一點時間跟 AI 進行了一場「Code Review 戰爭」。

我發現 AI 就像剛畢業的 Junior Engineer：
❌ 喜歡亂改變數名稱 (AaronWuBuilds 變成 AaronBuilds，導致 SSH 驗證失敗)
❌ 喜歡自作聰明加功能 (亂塞 Shortcode 導致 Hugo Build 失敗)

在這篇文章裡，我紀錄了我如何從「修正 AI 幻覺」到建立一套「輸入大綱 -> 自動產出中英文章 + 社群貼文」的自動化流程。

重點不是 AI 有多神，而是人類如何定義 Spec。

入伍前的腦袋備份計畫，正式啟動。👇

🔗 全文傳送門：https://aaronwubuilds.github.io/posts/blog-automation-workflow-zh/

#BuildInPublic #Automation #Python #DevOps #工程師 #當兵倒數

---

### 2. LinkedIn (English)
**Target:** Recruiters, Senior Engineers, Tech Leads
**Tone:** Professional, Insightful, Result-oriented

As engineers, we often talk about "Architecture." But have you ever applied architectural thinking to your content workflow?

With 20 days left before my mandatory military service, I needed a way to keep my technical blog alive without manual intervention. I turned to AI, but I quickly learned a valuable lesson:

**Human = Architect. AI = Executor.**

If you let AI define the workflow, you get hallucinations and feature creep. I spent days debugging "creative" variable name changes and unauthorized shortcode injections that broke my build pipeline.

I treated the AI output like a strict Code Review session. The result?
A Python-based workflow that takes a raw idea and compiles it into:
✅ SEO-optimized English Markdown (for Dev.to/Personal Blog)
✅ Localized Chinese Markdown
✅ Ready-to-post Social Media snippets

It reduced my publishing time from 4 hours to 30 minutes.

I wrote a detailed breakdown of how I designed this system—and more importantly, how I debugged the AI to adhere to strict engineering specs.

Read the full breakdown here: 👇
https://aaronwubuilds.github.io/posts/blog-automation-workflow-en/

#SoftwareEngineering #SystemDesign #Automation #GenerativeAI #DevOps #AaronWuBuilds

---

### 3. X / Twitter (English)
**Target:** Tech Twitter, Indie Hackers
**Tone:** Punchy, Thread-starter style

I have 20 days until military service. 🪖
I refuse to let my blog die while I'm away.

So I built a Python pipeline to automate it.
But here's the catch: AI is a terrible Architect. It's only a good Executor.

I had to fight AI hallucinations, random variable changes, and broken build scripts to get this working.

Here is how I reduced my publishing time from 4 hours to 30 mins:
🔗 https://aaronwubuilds.github.io/posts/blog-automation-workflow-en/

#BuildInPublic #Python #Automation #DevOps

```

### Part 4: AI Image Prompt

```markdown
**Prompt for Midjourney / DALL-E 3:**

An isometric, minimalist 3D illustration of a young software engineer sitting at a floating desk, diligently reviewing a holographic blue print layout of a pipeline. The pipeline connects a "Raw Idea" input on the left to multiple outputs on the right (documents, globes).

The style should be clean, tech-focused but not overly chaotic.
Colors: Slate blue, white, and a distinct "alert orange" highlighting a bug in the holographic code that the engineer is pointing at.
Background: A subtle digital grid fading into darkness.
Vibe: Precision, Architecture, "Human in control of the Machine".

```