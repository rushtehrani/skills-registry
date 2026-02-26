---
name: commit
description: Create a well-structured git commit following conventional commits. Use when the user says "commit this", "make a commit", or "commit my changes".
disable-model-invocation: true
allowed-tools: Bash(git *)
---

# Create a Git Commit

## Step 1: Understand What Changed

```bash
git status
git diff --staged
git diff
```

Review both staged and unstaged changes. If nothing is staged, ask the user which files to include.

## Step 2: Stage the Right Files

Stage only the files relevant to this logical change:
```bash
git add <specific-files>
```

Never use `git add -A` or `git add .` without first confirming with the user, as it may include unintended files (secrets, build artifacts, etc.).

## Step 3: Determine the Commit Type

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code restructure without behavior change |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `chore` | Build system, deps, config |
| `perf` | Performance improvement |
| `ci` | CI/CD pipeline changes |

## Step 4: Write the Commit Message

Follow **Conventional Commits** format:

```
<type>(<optional-scope>): <short summary in imperative mood>

<optional body: what and why, not how>

<optional footer: breaking changes, issue refs>
```

Rules:
- Summary line: ≤72 characters, imperative mood ("add" not "added")
- Body: wrap at 72 characters
- Reference issues: `Closes #123`, `Fixes #456`
- Breaking changes: `BREAKING CHANGE: <description>` in footer

## Step 5: Create the Commit

```bash
git commit -m "$(cat <<'EOF'
<type>(<scope>): <summary>

<body if needed>
EOF
)"
```

## Step 6: Confirm

Run `git log --oneline -5` to verify the commit was created correctly.
