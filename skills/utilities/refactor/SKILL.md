---
name: refactor
description: Refactor code for improved readability, maintainability, or structure without changing behavior. Use when asked to "clean this up", "refactor this", "improve this code", or "make this more readable".
argument-hint: [file-path or function-name]
allowed-tools: Read, Edit, Write, Glob, Grep, Bash
---

# Refactor: $ARGUMENTS

## Guiding Principle

**Refactoring must not change observable behavior.** If a change alters what the code does, it is not refactoring — it is modification.

Before touching anything, run existing tests to establish a green baseline.

## Step 1: Understand the Current Code

Read the code and understand:
- What it does (fully, not just approximately)
- Its callers and how they depend on it
- Its dependencies
- Existing tests

## Step 2: Identify Improvement Opportunities

Work through these categories:

### Naming
- Rename variables, functions, and classes to reveal intent
- Replace abbreviations with full words (unless domain-standard)
- Ensure names are consistent with the surrounding codebase

### Functions
- Extract long functions into smaller, named helpers
- Remove redundant parameters
- Replace boolean parameters with two well-named functions
- Eliminate side effects hidden inside functions that should be pure

### Structure
- Replace nested conditionals with early returns (guard clauses)
- Eliminate duplicate code with a shared abstraction
- Move methods to the class that owns the data they operate on
- Replace magic numbers/strings with named constants

### Complexity
- Simplify overly clever code — simple is better than clever
- Break complex boolean expressions into named variables
- Replace deeply nested structures with flat ones

### Dead code
- Remove unused variables, functions, imports, and commented-out code

## Step 3: Plan the Changes

List each refactoring move explicitly before making any edits. Order them from safest (rename) to most structural (extract class). This prevents getting lost mid-refactor.

## Step 4: Apply Changes Incrementally

Apply one change at a time. After each change:
- The code must still compile/parse
- Tests must still pass

Do not make multiple changes in a single step.

## Step 5: Verify

After all changes:
- Run the full test suite — all tests must pass
- Read the refactored code aloud (mentally) — is it clearer?
- Check that the public API is unchanged (same function signatures, same behavior)

## Step 6: Summarize

Report:
- What was changed and why
- Any behavior-adjacent observations (bugs, edge cases, dead code) found during refactoring — listed separately so the user can decide whether to address them
