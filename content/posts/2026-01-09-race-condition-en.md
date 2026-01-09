---
title: "[System Design] Why Is Your App 'Overselling'? A Founder's First Lesson in Concurrency"
date: 2026-01-09T12:00:00+08:00
draft: false
tags: ["System Design", "Startup", "Backend", "Redis"]
categories: ["Engineering"]
author: "Aaron"
description: "Stock was 10, but you sold 12? That's a Race Condition. Here is how to fix it using Redis atomic operations."
---

You're Bob, a bootstrapped founder who can't afford to hire a software engineer. In this era of "Vibe Coding," you believe you can build a Minimum Viable Product (MVP) just by relying on AI. 🤖

Today, you finally figured out how to set up an API, handle frontend rendering, and connect a SQL database. You actually built an e-commerce app! The tests passed, and it's finally time for the soft launch. 🚀

But the moment you launch your first marketing campaign, the users you worked so hard to acquire start complaining.

> "Bob! I clearly saw there was stock left. Why did you charge my card but not ship the item?!" 😠

You rush to the backend dashboard to check. You had 10 items in stock, but somehow, you sold 12. Where on earth are you going to find two extra pairs of limited-edition Jordans?

You start sweating bullets. 😓 You don't want anyone to find out you're just a frugal founder; you can't let your brand reputation crash right after takeoff!
You panic and run to ask your backend engineer friend, **Aaron**.

"Aaron, Aaron! What is going on? Is the universe against me? Did the computer just decide to add two extra numbers for fun?"

Aaron pats you on the back. "This isn't bad luck, Bob. This is a classic **Race Condition**."

"Race Condition?"

---

## 🛑 Why Your Code Lies to You

Bob, your logic probably looks something like this, right?

1.  **Read**: Check the database. Is there stock? (`Select count...`)
2.  **Logic**: If stock > 0, sell it to the user.
3.  **Write**: Subtract 1 from stock and update the database. (`Update...`)

This logic works perfectly when you are the only one testing it.
But when **User A** and **User B** press the "Buy" button at the **exact same millisecond**, a tragedy occurs:

1.  User A checks and sees the stock is 1.
2.  User B **simultaneously** checks and also sees the stock is 1 (because A hasn't finished the purchase yet).
3.  A completes the purchase, stock becomes 0.
4.  B **also thinks they completed the purchase**, stock becomes -1.

This is a **Race Condition**.

The most intuitive solution is to use a **"Lock" 🔒** on the database, forcing customer requests to queue up and be processed one by one.
However, writing to a hard drive (database) takes time. In a high-traffic scenario, your database simply cannot lock and unlock fast enough to hold back the zombie-like horde of users.

---

## ✅ The Industry Standard Solution: Atomic Operations

In professional backend architecture, we usually don't let the relational database (SQL) stand on the front lines for issues involving "money" and "quantity." Instead, we bring in a gatekeeper: **Redis**. 🛡️

We utilize a specific characteristic of Redis: **Single-threaded Atomicity**.

Imagine Redis as a **ticket booth with only one window**. 🎫
No matter if 10,000 people try to squeeze in at once, Redis forces everyone to stand in a single file line.

We use the Redis `DECR` (decrement) command:

* **User A arrives** ➡️ Redis says: Subtract 1, remaining is 0. **Success**.
* **User B arrives in the same millisecond** ➡️ Redis says: Get in line. By the time it's your turn, the remainder is 0. Subtracting 1 makes it -1. **Failed. Get out.**

You scratch your head. "Wait, isn't that the same as locking the database?"

Aaron shakes his head. "It is completely different! Redis stores all data in **RAM (memory)** and is highly optimized for this. The speed is on a completely different level!" ⚡

---

## 💡 Aaron's Practical Summary

Your eyes light up. "So, I just need to add Redis and my problems are solved?"

"Correct, but that is just the first step," Aaron says, taking a sip of coffee. ☕ "The point isn't the tool, it's the architectural mindset. You need to learn to distinguish between layers:

1.  **The Caching Layer (Redis)**: Used to handle high traffic, deflect requests, and determine instant sales results.
2.  **The Database Layer (DB)**: Used to store orders that have already been confirmed as successful."

You fall into deep thought. "I see... I only learned how to use the basic tools. I still have so much to learn."
Aaron nods. "Exactly. But how complex your architecture needs to be depends on your business. Everything is done to protect your commercial reputation."

After hearing the solution, you rush home to stay up all night fixing your code. Aaron thinks to himself, "I hope Bob doesn't crash the Redis memory this time..." But that is a story for another day.

---

### [Next]

You suddenly think: "Since Redis is so powerful, can I just throw away my SQL database?"
In the next post, we will discuss: **The risks of data persistence, and why we still need traditional databases.**

👇 **Want to see how I implement this anti-overselling system?**
I am Aaron, a software engineer based in the US. I am currently turning this architecture into a simple open-source project called **"FlashForm"** for reference.
If you enjoy these kinds of architectural notes that solve real business problems, feel free to follow and subscribe.

#SystemDesign #Startup #RaceCondition #Backend #AaronWuBuilds
