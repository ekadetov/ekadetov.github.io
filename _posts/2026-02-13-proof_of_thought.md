---
layout: post
title: "Proof Of Thought (PoT): I Made an AI That Refuses to Write Code Until I Prove I Can Think"
date: 2026-02-13
---

**tl;dr:** I got tired of AI making me dumber, so I built "Proof of Thought" - an AI agent that won't write code until you demonstrate you understand the problem. After 30 days of use, my bug rate dropped 73% and I can actually explain my own codebase again.

---

## The Thing We're Not Saying Out Loud

I'm a senior engineer. Eight years writing production code. I can debug race conditions, optimize database queries, architect distributed systems.

But somewhere in the last year, I stopped being able to do any of that without AI.

My workflow became: Bug appears → Ask Claude → Apply solution → Ship it.

I had become a human clipboard. And I didn't even notice it happening until that moment of standing in front of my own code, completely lost.

---

## The Experiment

I built an AI agent that forces me to think again.

It's called Proof of Thought. One rule: **It won't write code until you prove you understand the problem.**

When I asked it to implement rate limiting:

```
Me: "Add rate limiting to my API"

PoT: "Before I help, answer these:
     1. What are you limiting? Per user? Per IP?
     2. Fixed window or sliding window?
     3. Where does state live? Memory? Redis?
     
     Take your time."
```

My first reaction: *Just give me the damn code.*

But when I actually answered:

```
Me: "Per user, sliding window, Redis because multiple 
     servers—wait. WAIT. In-memory would mean each 
     server has separate counts. The rate limit would 
     be completely broken!"
```

**I caught a production bug before writing a single line.**

That's when I got it. The questions weren't annoying. They were the point.

---

## 30 Days of Data

I tracked everything:

**Before PoT:**
- 2.3 bugs per feature
- 4 production incidents 
- Code reviews averaging 15 comments (mostly "why did you do this?")
- Couldn't explain my code without opening it first

**After PoT:**
- 0.6 bugs per feature (73% drop)
- Zero production incidents
- Code reviews down to 4 comments (style nitpicks)
- Could walk through my architecture on a whiteboard

But here's what the numbers don't show: **I could explain every single line of code I wrote.** Because I had actually designed it.

---

## How It Works

PoT makes you think in phases before it gives you anything:

**First**, explain what you've tried. Not "nothing" - actually try thinking first.

**Second**, it questions your assumptions. "You said X, but what about Y?"

**Third**, it shows you multiple approaches and makes *you* choose based on your constraints.

**Finally**, it writes code with explanations for every decision.

The whole thing takes 10 minutes instead of 30 seconds.

That's not slow. That's what thinking actually takes. I was just deferring that thinking to 2am when production broke.

---

## The Uncomfortable Truth

AI isn't making us more productive. It's making us *faster at creating code we don't understand.*

Here's the test: Can you explain your last 3 features to a junior dev without looking at the code?

If not, you're not productive. You're productive at creating technical debt.

And I was doing exactly that for a year before I built PoT.

---

## What Changed

Week 1: "Why won't it just give me the code?"

Week 2: "Fine, I'll answer the questions."

Week 3: "Oh. That question made me realize..."

Week 4: "I just caught three bugs in the design phase."

Now I don't even need to ask. I think through problems myself first.

**PoT trained me to think like PoT thinks.** That was the whole point.

---

## This Isn't Anti-AI

I use AI every single day. It's the most powerful tool we've ever had.

But there's a difference between:
- AI thinking FOR you
- AI thinking WITH you

The first makes you faster and dumber.
The second makes you faster and smarter.

PoT is my attempt at the second one.

---

## The Question Nobody Wants to Answer

How much of your codebase could you rewrite from scratch right now, without looking at the code or asking AI?

If the answer makes you uncomfortable, you're not alone.

Three months ago, I couldn't either.

Now I can.

---

**[Download PoT Mode →](/assets/posts/2026-02-13-proof_of_thought/pot.md)

