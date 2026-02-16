---
name: opencode-upgrade
description: Upgrades OpenCode to the latest version via Homebrew. Use when the user wants to update OpenCode, check for updates, or ensure they have the latest version installed.
license: MIT License - See LICENSE.txt
---

# OpenCode Upgrade Skill

## Overview

This skill provides the complete workflow for upgrading OpenCode (installed via Homebrew) to the latest available version.

**Keywords**: OpenCode, upgrade, update, homebrew, brew, version, latest, install

## When to Use This Skill

Use this skill when:
- User asks to upgrade or update OpenCode
- User wants to check for OpenCode updates
- User wants to ensure they have the latest version
- User mentions OpenCode is outdated or has issues that may be fixed in a newer version

## Upgrade Process

### Complete Upgrade Command Sequence

```bash
brew update
TAP_DIR="$(brew --repo anomalyco/tap)"
git -C "$TAP_DIR" pull --rebase
brew upgrade anomalyco/tap/opencode
hash -r
opencode --version
```

### Step-by-Step Breakdown

#### 1. Update Homebrew Core
```bash
brew update
```
**Purpose**: Updates Homebrew itself and refreshes all formula definitions from the main repository.

**What it does**:
- Fetches the latest Homebrew updates
- Updates formula definitions
- Ensures you're working with current package information

#### 2. Locate the Tap Directory
```bash
TAP_DIR="$(brew --repo anomalyco/tap)"
```
**Purpose**: Stores the path to the `anomalyco/tap` repository in a variable.

**What it does**:
- Finds the exact location of the tap on your system
- Makes it easy to reference in subsequent commands
- Typically resolves to: `/opt/homebrew/Library/Taps/anomalyco/homebrew-tap` (Apple Silicon) or `/usr/local/Homebrew/Library/Taps/anomalyco/homebrew-tap` (Intel)

#### 3. Update the Tap Repository
```bash
git -C "$TAP_DIR" pull --rebase
```
**Purpose**: Fetches the latest OpenCode formula from the tap repository.

**What it does**:
- Changes to the tap directory
- Pulls the latest changes from the remote repository
- Uses rebase to keep a clean commit history
- Ensures you have the most recent formula definition

**Note**: This step is critical because `brew update` doesn't always fetch the latest changes from custom taps immediately.

#### 4. Upgrade OpenCode
```bash
brew upgrade anomalyco/tap/opencode
```
**Purpose**: Installs the latest version of OpenCode.

**What it does**:
- Downloads the new version if available
- Installs the updated binary
- Preserves your configuration files
- Links the new version to your PATH

**Output**:
- If already up-to-date: "opencode <version> is already installed"
- If upgrading: Shows download progress and installation status

#### 5. Refresh Shell Hash Table
```bash
hash -r
```
**Purpose**: Clears the shell's command hash table.

**What it does**:
- Forces the shell to re-scan the PATH
- Ensures the new `opencode` binary is found
- Prevents issues where old version is cached

**Why needed**: Some shells cache the location of executables for performance. This ensures you're using the freshly installed version.

#### 6. Verify the Installation
```bash
opencode --version
```
**Purpose**: Confirms the upgrade was successful and shows the new version.

**What it does**:
- Displays the currently installed OpenCode version
- Provides immediate feedback that the upgrade worked
- Allows you to confirm you have the expected version

## Quick Reference

### One-Liner Version
For advanced users, all commands can be run in sequence:

```bash
brew update && TAP_DIR="$(brew --repo anomalyco/tap)" && git -C "$TAP_DIR" pull --rebase && brew upgrade anomalyco/tap/opencode && hash -r && opencode --version
```

### Check Current Version Only
```bash
opencode --version
```

### Check if Update is Available
```bash
brew outdated anomalyco/tap/opencode
```

## Troubleshooting

### Common Issues

#### "Error: anomalyco/tap/opencode not installed"
**Solution**: Install OpenCode first:
```bash
brew tap anomalyco/tap
brew install anomalyco/tap/opencode
```

#### "fatal: not a git repository"
**Solution**: The tap may be corrupted. Re-add the tap:
```bash
brew untap anomalyco/tap
brew tap anomalyco/tap
brew install anomalyco/tap/opencode
```

#### "Permission denied" errors
**Solution**: Fix Homebrew permissions:
```bash
sudo chown -R $(whoami) $(brew --prefix)/*
```

#### Version didn't change after upgrade
**Solution**:
1. Check if upgrade actually occurred: `brew upgrade anomalyco/tap/opencode --verbose`
2. Force reinstall: `brew reinstall anomalyco/tap/opencode`
3. Restart your terminal
4. Run `hash -r` again

#### "Already up-to-date" but you know a new version exists
**Solution**: The tap cache may be stale:
```bash
cd "$(brew --repo anomalyco/tap)"
git fetch --all
git reset --hard origin/main  # or origin/master depending on default branch
brew upgrade anomalyco/tap/opencode
```

## Additional Information

### Configuration Preservation
The upgrade process preserves:
- OpenCode configuration files
- User preferences
- Installed extensions (if any)
- Custom settings

### Rollback
If you need to revert to a previous version:
```bash
brew uninstall anomalyco/tap/opencode
brew install anomalyco/tap/opencode@<version>
```

### Stay Updated
To check for updates regularly:
```bash
# Add to your shell profile (.bashrc, .zshrc, etc.)
alias opencode-check="brew outdated anomalyco/tap/opencode"
```

## System Requirements

- macOS (Apple Silicon or Intel)
- Homebrew installed
- Active internet connection
- Git (usually comes with Xcode Command Line Tools)

## Getting Help

If you encounter issues not covered here:
1. Check OpenCode documentation: https://opencode.ai/docs
2. Review Homebrew logs: `brew --prefix`/var/log/
3. Contact OpenCode support
4. Report tap issues: https://github.com/anomalyco/homebrew-tap/issues

## Related Commands

```bash
# Check all outdated packages
brew outdated

# See what will be upgraded
brew upgrade --dry-run

# Get info about OpenCode package
brew info anomalyco/tap/opencode

# List all installed versions
brew list --versions opencode
```