---
name: skills-sync
description: Sync the ~/.claude/skills/ folder with GitHub. Use when the user wants to push local skill changes to GitHub, pull skill updates from GitHub, or check sync status. Triggers on phrases like "sync skills", "push skills", "pull skills", "update skills from github".
---

# Skills Sync

Sync the Claude skills folder (`~/.claude/skills/`) with GitHub remote.

## Prerequisites

- Skills folder is a git repo: `~/.claude/skills/.git/` exists
- Remote origin: `https://github.com/colinleefish/agent-skills.git`

## Network Configuration

Always use HTTPS with proxy:

```bash
https_proxy=localhost:1081 git <command>
```

## Operations

### Check Status

```bash
cd ~/.claude/skills && git status
```

### Pull

```bash
cd ~/.claude/skills && https_proxy=localhost:1081 git pull
```

### Push

```bash
cd ~/.claude/skills && git add . && git commit -m "<message>" && https_proxy=localhost:1081 git push
```

### Full Sync (Pull → Push)

```bash
cd ~/.claude/skills && https_proxy=localhost:1081 git pull && git add . && git commit -m "<message>" && https_proxy=localhost:1081 git push
```

## Conflict Resolution

When `git pull` reports conflicts:

1. List conflicted files:
   ```bash
   git diff --name-only --diff-filter=U
   ```

2. For each conflicted file, read and examine both versions (marked by `<<<<<<<`, `=======`, `>>>>>>>`).

3. **Resolution strategy**: Keep the best and most useful content from both versions. Prefer:
   - More complete/comprehensive content
   - More recent improvements
   - Combine non-overlapping additions from both sides

4. Edit the file to remove conflict markers and keep the resolved content.

5. Stage and commit:
   ```bash
   git add <file> && git commit -m "Resolve conflict: <file>"
   ```

## Commit Message Conventions

- `Add: <skill-name>` — new skill
- `Update: <skill-name>` — modify existing
- `Remove: <skill-name>` — delete skill
- `Fix: <skill-name>` — bug fix
