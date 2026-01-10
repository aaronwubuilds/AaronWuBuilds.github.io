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