---
name: code-review
description: Perform a thorough code review. Use when asked to review code, a PR, a diff, or when the user asks "can you review this?" or "LGTM?". Checks for correctness, security, performance, and style.
argument-hint: [pr-number or file-path]
allowed-tools: Read, Glob, Grep, Bash(gh *)
---

# Code Review

## What to Review

$ARGUMENTS

If a PR number is given, fetch the diff:
```
gh pr diff $ARGUMENTS
gh pr view $ARGUMENTS --comments
```

If a file path is given, read that file directly.

---

## Review Checklist

Work through each category systematically.

### Correctness
- Does the code do what it claims to do?
- Are there off-by-one errors, null pointer risks, or race conditions?
- Are all error paths handled properly?
- Does it handle empty inputs, zero values, and boundary conditions?

### Security
- Is user input sanitized and validated?
- Are there SQL injection, XSS, or command injection risks?
- Are secrets hardcoded or logged?
- Are permissions and authorization checks correct?
- Are dependencies using known-vulnerable versions?

### Performance
- Are there N+1 query patterns or unnecessary loops?
- Is expensive work done inside hot paths?
- Are large allocations or copies avoidable?
- Is caching used appropriately?

### Maintainability
- Is the code readable and self-documenting?
- Are functions and classes doing one thing?
- Is there code duplication that should be extracted?
- Are variable and function names clear?

### Tests
- Do tests cover the happy path, error paths, and edge cases?
- Are tests isolated and deterministic?
- Is the test setup overly complex?

### API Design (if applicable)
- Is the public API intuitive and consistent?
- Are breaking changes flagged?
- Is backward compatibility maintained?

---

## Output Format

Structure your review as:

**Summary**: One paragraph overall assessment.

**Must Fix** (blocking issues):
- File:line — Description of the issue and why it matters

**Should Fix** (non-blocking improvements):
- File:line — Description

**Suggestions** (optional polish):
- File:line — Description

**Praise**: Call out what was done well.
