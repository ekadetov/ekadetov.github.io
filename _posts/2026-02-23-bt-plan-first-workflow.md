---
layout: post
title: "bt: Plan-First AI Coding for OpenCode"
date: 2026-02-23
---

**tl;dr:** I built five OpenCode commands that enforce a simple discipline—no code gets written until you've reviewed and approved a written plan.

---

## The Problem with AI Coding

AI tools are eager to write code. Too eager. You describe a feature, and within seconds you're looking at hundreds of lines of generated code. Sometimes it's great. Often it's subtly wrong in ways you won't notice until later.

The issue isn't capability—it's process. The AI doesn't know your product priorities, your users' pain points, or which trade-offs you're willing to make. It optimizes for "looks like working code" rather than "fits this specific system."

## Boris Tane's Insight

Boris Tane [wrote about his workflow](https://sanity.io/blog/how-i-use-claude-code) for AI-assisted development. The key insight: **plans are shared mutable state**. Instead of trying to steer the AI through chat messages, you both edit the same markdown document.

The workflow has distinct phases:

1. **Research** — AI reads code deeply, writes findings
2. **Plan** — AI proposes approach, you review
3. **Annotate** — You add inline notes, AI updates plan (repeat 1-6x)
4. **Implement** — AI executes the approved plan

The annotation cycle is where your judgment enters. It's cheap compared to undoing a bad implementation.

## The Commands

I packaged this workflow into five OpenCode commands:

```
/bt-research → /bt-plan → /bt-review (repeat) → /bt-implement
```

Each command produces or refines a markdown artifact stored in `.ai/tasks/`, so nothing gets overwritten between tasks.

### /bt-research

Deep-reads a codebase area and writes findings to `research.md`:

```
/bt-research src/services/notifications
/bt-research the task scheduling flow — look for bugs
```

### /bt-plan

Creates an implementation plan based on the research:

```
/bt-plan add cursor-based pagination to the list endpoint
```

### /bt-review

This is where the magic happens. You open `plan.md` and add notes directly:

- `NOTE: use drizzle:generate for migrations, not raw SQL`
- `NOTE: no — this should be a PATCH, not a PUT`
- `NOTE: remove this section entirely`

Then run `/bt-review`. The AI addresses every note and cleans up the document. Repeat until the plan is right.

### /bt-implement

Executes the approved plan, marking tasks complete as it goes.

---

## Installation

The commands live in `~/.config/opencode/commands/` (global, shared across projects).

**Let the agent install them:**

```
Install the bt commands from https://github.com/ekadetov/bt-opencode-commands
```

**Or manually:**

```bash
git clone https://github.com/ekadetov/bt-opencode-commands.git /tmp/bt-commands
cp /tmp/bt-commands/commands/*.md ~/.config/opencode/commands/
rm -rf /tmp/bt-commands
```

---

## Why This Works

Three things make this workflow effective:

**1. Forced slowdown.** The phases create natural checkpoints. You can't skip to implementation without going through research and planning.

**2. Written artifacts.** `plan.md` survives context compaction. By the time you say `/bt-implement`, the AI has spent the entire conversation building understanding, and the plan document captures it.

**3. Your judgment enters via annotation.** The AI is good at understanding code and proposing solutions. You know what actually matters for your users. The annotation cycle merges both.

---

## When to Skip Phases

Not every task needs the full flow:

| Task | Start at |
|------|----------|
| New feature or architectural change | `/bt-research` |
| Well-understood change | `/bt-plan` |
| Bug fix where you know the cause | `/bt-plan` or chat |
| Typo, config change | Just type it |

The workflow scales down. Use judgment.

---

**Project:** [github.com/ekadetov/bt-opencode-commands](https://github.com/ekadetov/bt-opencode-commands)

Based on [How I Use Claude Code](https://sanity.io/blog/how-i-use-claude-code) by Boris Tane.
