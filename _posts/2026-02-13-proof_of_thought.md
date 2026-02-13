---
layout: post
title: "ProofOfThought (PoT): I Made an AI That Refuses to Write Code Until I Prove I Can Think"
date: 2026-02-13
---

**tl;dr:** I got tired of AI making me dumber, so I built "Proof of Thought" - an AI agent that won't write code until you demonstrate you understand the problem. After 30 days of use, my bug rate dropped 73% and I can actually explain my own codebase again.

---

## The Problem Nobody's Talking About

Three months ago, I shipped a feature using Claude. It worked perfectly. I was proud.

Two weeks later, a user reported a bug. I opened my own code and **had no idea what it did**.

I had to ask Claude to explain *my own code* back to me.

That was my "oh shit" moment.

---

## The Uncomfortable Truth

We're creating a generation of developers who can prompt but can't problem-solve.

I'm not talking about junior devs. I'm talking about *me* - a senior engineer with 8 years of experience who suddenly couldn't debug without an AI holding my hand.

Here's what I caught myself doing:

```
Bug appears → Ask AI what's wrong → Copy fix → Ship
```

I had become a **human clipboard**.

The scary part? I didn't even notice it happening.

---

## The Experiment

I decided to build an AI that would *force* me to think again.

I call it "Proof of Thought" (PoT). It's an OpenCode agent that follows one simple rule:

**Before it writes any code, you must prove you understand the problem.**

Here's what happened when I asked it to implement rate limiting:

```
Me: "I need to add rate limiting to my API"

PoT: "Before I help, answer these:
     1. What are you rate limiting? (per user/IP/endpoint?)
     2. What's your strategy? (fixed/sliding window?)
     3. Where does state live? (memory/Redis/DB?)
     4. What happens when limit is hit?
     
     Take your time."
```

My first reaction: *"Just give me the damn code."*

But here's what happened when I actually answered the questions:

```
Me: "Per user, sliding window, Redis because we have 
     multiple servers—wait. WAIT. If I use in-memory, 
     each server would have separate counts and the rate 
     limit would be completely broken!"
```

**I caught a production bug before writing a single line of code.**

---

## The Results (30 Days)

I tracked every feature I built for a month. Here's the data:

### Before PoT:
- **Bug rate:** 2.3 bugs per feature (average)
- **Time to explain my code:** "Uh... let me look at it again..."
- **Code reviews:** 15 comments average (mostly architecture questions)
- **Production incidents:** 4 (all from code I didn't understand)

### After PoT:
- **Bug rate:** 0.6 bugs per feature (73% reduction)
- **Time to explain my code:** Immediate (I designed it)
- **Code reviews:** 4 comments average (mostly style nitpicks)
- **Production incidents:** 0

But the real difference wasn't the metrics. It was this:

**I could explain every line of code I wrote.**

---

## How It Works

PoT uses a simple protocol:

### Phase 1: Understanding
Forces you to explain what you've tried and what you think the solution looks like.

### Phase 2: Challenge
Questions your assumptions. *"You mentioned X, but have you considered Y?"*

### Phase 3: Diagram
Makes you visualize the solution before coding. (This alone catches 50% of bugs)

### Phase 4: Alternatives
Shows multiple approaches, makes *you* choose based on trade-offs.

### Phase 5: Code
Only after all of the above, it writes code - with explanations for every decision.

---

## The Modes

I built three variants:

**🍯 Standard PoT** - Balanced. Challenges you but stays helpful.

**🔥 Strict Mode** - Refuses to write any code. Only gives you architectural hints and makes you implement it. (Yes, I actually use this.)

**🎓 Mentor Mode** - Patient teacher. Still makes you think, but with more scaffolding.

---

## The Uncomfortable Part

PoT is **slower** than normal AI.

- Regular AI: 30 seconds to get code
- PoT: 5-10 minutes of back-and-forth

But here's what I realized:

That's not "slow" - that's **what thinking actually takes**.

I wasn't saving time before. I was *deferring debugging time to production*.

Now I spend 10 minutes thinking upfront and save 2 hours of debugging later.

---

## The Most Controversial Feature: Roasting

When you paste code without context, PoT roasts you:

```
🔥 Hold on.

I see you pasted 200 lines of code without explanation.

Before I help:
1. What is this code supposed to do?
2. What's the actual problem?
3. What have you tried?

I'm not being difficult - I'm making sure you learn.
Take your time.
```

First time this happened to me, I was annoyed.

Then I realized: **I had just pasted AI-generated code that I didn't understand.**

The roast was justified.

---

## What I Learned

### 1. AI Is Making Us Intellectually Lazy

Not intentionally. But when the path of least resistance is "ask AI," we stop exercising our problem-solving muscles.

### 2. The Struggle IS the Point

Learning happens in the friction between not-knowing and figuring-it-out.

AI that removes all friction removes all learning.

### 3. You Can't Debug What You Don't Understand

Production bugs don't wait for you to ask AI for help.

You need to be able to reason about your code independently.

### 4. Prompting ≠ Engineering

Being good at prompting AI doesn't make you a better engineer.

It makes you a better prompt engineer.

Those are different skills.

---

## The Counterargument

I know what you're thinking: *"But AI makes me more productive!"*

Does it?

Or does it make you *faster at writing code you don't understand*?

Here's the test:

**Can you explain your last 3 features to a junior dev without looking at the code?**

If not, you're not being productive. You're being *productive at creating technical debt*.

---

## How to Try It

I open-sourced PoT. It's 5 markdown files you drop into `.opencode/agents/`.

No config. No setup. Just download and use.

**GitHub:** [github.com/yourusername/pot-opencode]

Three modes:
- `pot` - Standard
- `pot-strict` - Hardcore (my favorite)
- `pot-mentor` - Learning mode

And two subagents:
- `@pot-explorer` - Teaches you to navigate codebases
- `@pot-reviewer` - Makes you review your own code first

---

## The Bigger Picture

This isn't about being anti-AI.

I love AI. I use it every day. It's the most powerful tool we've ever had.

But tools can be used well or poorly.

**Using AI to think FOR you:** Poorly  
**Using AI to think WITH you:** Well

PoT is my attempt at the latter.

---

## What Happens Next

I've been using PoT for 30 days. Here's what I noticed:

### Week 1: Frustrating
*"Why won't it just give me the code?"*

### Week 2: Annoying
*"Fine, I'll answer the questions."*

### Week 3: Interesting
*"Oh, that question made me realize..."*

### Week 4: Powerful
*"I caught three bugs in the design phase."*

Now I don't even need to ask. I think through problems myself.

**PoT trained me to think like PoT thinks.**

That's the whole point.

---

## The Uncomfortable Question

How much of your codebase could you rewrite from scratch, right now, without looking at the code or asking AI?

If the answer is "not much," you have a problem.

And I had that problem three months ago.

PoT fixed it.

---

## Try It

Download PoT, use it for a week, and track:
1. How many bugs you catch in design phase
2. How well you can explain your code
3. How confident you feel shipping

Then tell me I'm wrong.

**I dare you.** 🍯

---

## Comments Section Preview

I know what you're going to say:

**"This is just Socratic method for coding"**  
Yes. That's the point. It works.

**"This sounds slow and annoying"**  
So is debugging production at 2am.

**"I already think before I code"**  
Great! PoT will be easy for you then.

**"AI is a tool, use it however you want"**  
Sure. But some ways make you better, some make you worse.

**"This is gatekeeping"**  
No, this is engineering. Understanding your code isn't optional.

---

**Discuss on HackerNews →**

*(Published: February 13, 2026)*