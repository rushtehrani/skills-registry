---
name: pr-summary
description: Summarize a pull request for reviewers or for writing a PR description. Use when asked to "summarize this PR", "write a PR description", or "what does this PR do?".
argument-hint: [pr-number]
context: fork
agent: Explore
allowed-tools: Read, Grep, Glob, Bash(gh *), Bash(git *)
---

# Summarize Pull Request $ARGUMENTS

## Context Gathering

!`gh pr view $ARGUMENTS`
!`gh pr diff $ARGUMENTS`
!`gh pr view $ARGUMENTS --comments`
!`gh pr diff $ARGUMENTS --name-only`

---

## Your Task

Analyze the pull request and produce a clear, concise summary.

### Output Format

**Title**: (suggest an improved PR title if the current one is vague)

**What this PR does** (2-4 bullet points):
- High-level description of each logical change

**Why** (motivation):
- Problem being solved or feature being added
- Link to related issues if visible

**Key changes by file/module**:
- `path/to/file.ts` — What changed and why
- (list all significantly changed files)

**Testing**:
- What tests were added or modified
- Manual testing steps (if visible from PR description or comments)

**Review focus areas**:
- Highlight the parts that need closest attention
- Note any trade-offs or TODOs left in the code

**Breaking changes** (if any):
- List any API changes, schema migrations, or behavior changes that affect consumers

---

Keep the summary factual and based on the diff. Do not add speculation about intent unless supported by the PR description or comments.
