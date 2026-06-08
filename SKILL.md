---
name: yunfei-helloagents-worktree-sop
description: Use when starting or finishing repository work with a personal standard flow that should combine git worktree isolation, HelloAGENTS execution, and branch-finish cleanup without modifying HelloAGENTS managed files.
---

# Yunfei HelloAGENTS Worktree SOP

## Overview

Use this skill to enforce a stable personal workflow around HelloAGENTS without editing HelloAGENTS-managed files such as `~/.codex/AGENTS.md` or `~/.helloagents/helloagents/...`.

Core principle:

1. First isolate work with git branch or worktree
2. Then execute the task through HelloAGENTS
3. Then complete HelloAGENTS closeout
4. Only after that finish the branch lifecycle

This skill is a personal wrapper around existing capabilities. It does not replace HelloAGENTS. It standardizes how to enter and exit work.

## When to Use

Use when any of the following is true:

- starting a new feature, bugfix, or refactor and wanting an isolated worktree first
- wanting to follow a fixed personal SOP with HelloAGENTS
- finishing implementation and needing the correct order for `~qa`, `~commit`, `~clean`, and branch integration
- deciding whether to merge, create a PR, keep the branch, or discard work
- trying to avoid forgetting the correct start or finish order

Do not use when:

- the request is only a simple read-only question
- no repository changes will be made
- the user explicitly wants raw Git commands only without HelloAGENTS workflow

## Fixed Workflow

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

## Mode Selection

### Start Mode

Use this when the user is beginning work.

Follow this order:

1. Check whether the current task should be isolated from the current workspace
2. If isolation is needed, use `using-git-worktrees`
3. After entering the new worktree or branch, move into HelloAGENTS workflow
4. Choose the HelloAGENTS entry based on task type:
   - `~plan` for unclear or larger work
   - `~build` for clear small implementation
   - `~loop` for long-running work
5. Treat the current branch name as the active workspace context for HelloAGENTS state

Default guidance:

- prefer creating a dedicated feature branch
- prefer a dedicated worktree for medium or large changes
- do not start implementation on `main` or `master` unless the user explicitly wants that

### Execution Mode

Use this while work is in progress.

Rules:

- implementation changes should stay inside the isolated branch or worktree
- HelloAGENTS handles planning, execution, verification, state, and closeout evidence
- do not jump to PR or merge before HelloAGENTS closeout is complete

Typical commands:

- `~init`
- `~plan`
- `~build`
- `~loop`
- `~qa`

### Finish Mode

Use this when implementation is done or nearly done.

Always follow this order:

1. `~qa`
2. `~commit`
3. `~clean`
4. `finishing-a-development-branch`

Interpretation:

- `~qa` = quality gate and verification
- `~commit` = local commit
- `~clean` = archive finished HelloAGENTS plan artifacts and clean runtime leftovers
- `finishing-a-development-branch` = Git integration decision

Never reverse this order.

Do not create a PR, merge, delete a branch, or remove a worktree before HelloAGENTS closeout is complete unless the user explicitly overrides the SOP.

## Branch Finish Policy

When `finishing-a-development-branch` is used, interpret the choices with this policy:

1. Merge back locally
   - allowed only after tests and verification pass
   - after merge, branch cleanup is allowed
   - worktree cleanup is allowed

2. Push and create a Pull Request
   - allowed only after tests and verification pass
   - keep the branch
   - keep the worktree by default unless the user explicitly wants cleanup

3. Keep the branch as-is
   - keep branch
   - keep worktree

4. Discard the work
   - require explicit confirmation
   - only then allow destructive cleanup

Default preference:

- if collaboration or review is expected, prefer PR
- if the user is working solo and wants immediate local integration, local merge is acceptable
- if there is uncertainty, keep branch and worktree rather than deleting early

## Operational Rules

### Rule 1: Do not modify HelloAGENTS managed files

Do not propose storing this SOP inside:

- `~/.codex/AGENTS.md`
- `~/.helloagents/helloagents/...`
- HelloAGENTS built-in command skills
- HelloAGENTS managed runtime files

This SOP belongs in a personal skill only.

### Rule 2: Git isolation comes before HelloAGENTS execution

If starting implementation work and the current repository context is not isolated, first establish branch or worktree isolation.

### Rule 3: HelloAGENTS closeout comes before Git integration

Always prefer:

```text
~qa → ~commit → ~clean
```

before:

```text
merge / PR / delete branch / remove worktree
```

### Rule 4: Keep state stable

Avoid starting in one branch and moving to another branch or worktree halfway through a task, because HelloAGENTS state and evidence are scoped to the active workspace.

## Recommended Response Patterns

### If the user is starting a task

Respond with guidance equivalent to:

1. create or enter an isolated worktree or feature branch
2. run `~init`
3. choose `~plan`, `~build`, or `~loop` based on scope

### If the user is finishing a task

Respond with guidance equivalent to:

1. run `~qa`
2. run `~commit`
3. run `~clean`
4. then run branch-finish workflow

### If the user asks for the shortest reminder

Respond with this compressed SOP:

```text
先开 worktree/branch，再跑 HelloAGENTS，完成后先 ~qa ~commit ~clean，最后再做 branch finish。
```

## Quick Reference

### Start a new task

```text
using-git-worktrees
→ ~init
→ ~plan or ~build or ~loop
```

### Finish a task

```text
~qa
→ ~commit
→ ~clean
→ finishing-a-development-branch
```

### Mental model

- Git creates the workspace
- HelloAGENTS completes the task
- branch finish integrates or cleans up the code path

## Common Mistakes

### Starting work on `main`

Problem:
- state and implementation happen in the wrong workspace

Fix:
- isolate first with branch or worktree

### Creating PR before `~qa`

Problem:
- review starts before quality loop and evidence are complete

Fix:
- finish HelloAGENTS closeout first

### Cleaning branch/worktree too early

Problem:
- context and recovery path disappear too early

Fix:
- only clean after the finish decision is explicit

### Editing HelloAGENTS managed files to store personal SOP

Problem:
- updates may overwrite the customization

Fix:
- keep the SOP in this personal skill only
