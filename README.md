# yunfei-helloagents-worktree-sop

Personal Codex skill for enforcing a stable repository workflow around:

- `using-git-worktrees`
- HelloAGENTS execution
- `finishing-a-development-branch`

The goal is simple:

1. isolate work first with a branch or worktree
2. run the task through HelloAGENTS
3. finish HelloAGENTS closeout first
4. only then handle PR / merge / branch cleanup

## Main flow

```text
using-git-worktrees
→ enter isolated branch/worktree
→ ~init
→ ~plan / ~build / ~loop
→ ~qa
→ ~commit
→ ~clean
→ finishing-a-development-branch
```

## Why this exists

HelloAGENTS itself is managed and updated regularly. This repository keeps a personal workflow skill outside the HelloAGENTS managed runtime, so the SOP is not overwritten by normal HelloAGENTS updates.

## Skill file

The main skill lives in:

- `SKILL.md`
