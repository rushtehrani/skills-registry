---
name: fix-issue
description: Fix a GitHub issue end-to-end. Use when a user provides an issue number or URL and wants the issue resolved with code changes, tests, and a proper commit.
argument-hint: [issue-number]
disable-model-invocation: true
allowed-tools: Read, Edit, Write, Glob, Grep, Bash(gh *), Bash(git *)
---

# Fix GitHub Issue $ARGUMENTS

## Step 1: Understand the Issue

Fetch and read the issue details:

```
gh issue view $ARGUMENTS
```

Read the issue body, labels, comments, and any linked PRs for full context.

## Step 2: Explore the Codebase

Before writing any code:
- Identify the files and modules relevant to this issue
- Read existing tests to understand expected behavior
- Look for related patterns or utilities already in the codebase
- Check if there are any open PRs addressing related issues

## Step 3: Plan the Fix

Create a clear plan:
1. Root cause analysis — what exactly is broken or missing?
2. Proposed solution — minimal change that solves the issue
3. Edge cases — what could go wrong?
4. Test strategy — how will you verify the fix?

## Step 4: Implement

- Make the smallest change that fully resolves the issue
- Follow existing code style, patterns, and conventions in the codebase
- Do NOT refactor unrelated code
- Do NOT add features beyond what the issue requires

## Step 5: Write Tests

- Add or update tests that would have caught this issue
- Ensure existing tests still pass
- Cover edge cases identified in Step 3

## Step 6: Verify

Run the project's test suite and linters. Fix any failures before proceeding.

## Step 7: Commit

Create a commit with:
- Title: `fix: <concise description of what was fixed> (closes #$ARGUMENTS)`
- Body: Brief explanation of root cause and approach

Reference the issue number so GitHub auto-closes it on merge.
