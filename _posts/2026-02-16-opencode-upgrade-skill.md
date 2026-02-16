---
layout: post
title: "OpenCode Upgrade Skill: Automating Updates with Zero Downtime"
date: 2026-02-16
---

**tl;dr:** I built an upgrade skill for OpenCode that handles the entire update workflow—from refreshing Homebrew to verifying the new version—in one command.

---

## The Complete Upgrade Sequence

The skill I built automates the entire process:

```bash
brew update
TAP_DIR="$(brew --repo anomalyco/tap)"
git -C "$TAP_DIR" pull --rebase
brew upgrade anomalyco/tap/opencode
hash -r
opencode --version
```

Each step serves a specific purpose:

**1. `brew update`**
Refreshes Homebrew's core formula definitions. Still necessary even though it doesn't catch custom tap updates.

**2. Locate the tap directory**
Different systems store taps in different locations:
- Apple Silicon: `/opt/homebrew/Library/Taps/...`
- Intel: `/usr/local/Homebrew/Library/Taps/...`

Using `brew --repo` ensures we get the right path regardless of system.

**3. Force-update the tap**
This is the critical step that `brew update` misses. We pull directly from the tap's git repository with `--rebase` to maintain a clean history.

**4. Upgrade the package**
Now that we have the latest formula, actually perform the upgrade.

**5. Refresh the shell hash table**
Most shells cache executable locations for performance. Without this, you might keep running the old version even after upgrading.

**6. Verify**
Confirm the upgrade worked and show the new version number.

---

## The Full Skill Definition

For those interested in the technical details or building similar skills, here's the complete skill definition:

**File:** [`SKILL.md`](https://github.com/ekadetov/ekadetov.github.io/blob/main/assets/posts/2026-02-16-opencode-upgrade-skill/opencode-upgrade/SKILL.md)
