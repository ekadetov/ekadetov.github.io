# bt — Plan-First AI Coding for OpenCode

Five commands that enforce a simple rule: **no code gets written until you've reviewed and approved a written plan.**

```
/bt-research → /bt-plan → /bt-review (1-6x) → /bt-implement
```

Based on [Boris Tane's workflow](https://sanity.io/blog/how-i-use-claude-code).

## Installation

### Option 1: Let the agent install it

Copy this prompt and paste it into OpenCode:

```
Install the bt commands from https://github.com/ekadetov/bt-opencode-commands

1. Clone the repo to a temp location
2. Copy all .md files from commands/ to ~/.config/opencode/commands/
3. Verify with: ls ~/.config/opencode/commands/bt-*.md
4. Clean up the temp clone
```

### Option 2: Manual install

```bash
git clone https://github.com/ekadetov/bt-opencode-commands.git /tmp/bt-commands
mkdir -p ~/.config/opencode/commands
cp /tmp/bt-commands/commands/*.md ~/.config/opencode/commands/
rm -rf /tmp/bt-commands
```

### Option 3: One-liner

```bash
curl -fsSL https://raw.githubusercontent.com/ekadetov/bt-opencode-commands/main/install.sh | bash
```

## Commands

| Command | Purpose |
|---------|---------|
| `/bt-research <target>` | Deep-read code, write findings to `.ai/tasks/` |
| `/bt-plan <description>` | Write implementation plan |
| `/bt-review` | Process your inline annotations |
| `/bt-todo` | Generate task checklist |
| `/bt-implement` | Execute the plan |
| `/bt-help` | Show workflow guide |

## Usage

```bash
/bt-help                                    # First time — read the guide

/bt-research src/services/auth              # Understand the code
# → review .ai/tasks/.../research.md

/bt-plan add rate limiting to login         # Get a plan
# → review plan.md, add NOTE: comments

/bt-review                                  # AI processes your notes
# → repeat 1-6 times

/bt-implement                               # Execute when plan is right
```

## Why this exists

AI coding tools are eager to write code. That's the problem. The best results come from:

1. Making the AI understand deeply first
2. Getting a written plan you can review
3. Adding your domain knowledge via annotations
4. Only then letting it implement

This workflow makes that discipline automatic.

## License

MIT
