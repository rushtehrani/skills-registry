---
name: debug
description: Systematically debug an error or unexpected behavior. Use when the user shares an error message, stack trace, or says "this isn't working", "I'm getting an error", or "help me debug".
allowed-tools: Read, Glob, Grep, Bash
---

# Debug Session

## Step 1: Gather Information

Before guessing, collect the full picture:

1. **The error**: Exact error message and stack trace (request it if not provided)
2. **Reproduction**: Steps to reproduce — what exactly triggers the error?
3. **Expected vs actual**: What should happen? What actually happens?
4. **Environment**: Language version, OS, relevant dependencies and their versions
5. **Recent changes**: What changed before this started happening?

## Step 2: Read the Stack Trace

Work through the stack trace from bottom to top:
- The bottom frame is where execution started
- The top frame is where the crash occurred
- Find the first frame in **your own code** (not a library) — that's where to look first

## Step 3: Form Hypotheses

List possible root causes, most likely first. Common categories:

| Category | Examples |
|----------|---------|
| Null/undefined | Missing field, uninitialized variable |
| Type mismatch | String vs number, wrong shape |
| Off-by-one | Loop bounds, index, slice |
| Race condition | Async code, concurrency |
| Config/env | Missing env var, wrong URL, wrong credentials |
| Dependency | Version mismatch, breaking change in library |
| Logic error | Wrong condition, incorrect algorithm |

## Step 4: Test Hypotheses

For each hypothesis, find evidence for or against:
- Read the relevant code
- Add temporary logging/print statements if needed
- Narrow down with a minimal reproduction case

Work from most likely to least likely. Stop when you find evidence.

## Step 5: Fix

Once root cause is confirmed:
- Make the minimal change that fixes it
- Do not refactor surrounding code unless it's directly related
- Explain why the fix works, not just what it does

## Step 6: Prevent Recurrence

Suggest:
- A test that would have caught this issue
- A linter rule or type annotation that prevents this class of bug
- Documentation or comment to make the behavior clearer

---

## Debugging Rules

1. **Never guess and patch** — understand before fixing
2. **One hypothesis at a time** — changing multiple things at once obscures the cause
3. **Reproduce first** — if you can't reproduce it, you can't know if it's fixed
4. **Read the error message** — the answer is often right there
