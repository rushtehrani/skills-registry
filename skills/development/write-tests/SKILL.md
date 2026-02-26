---
name: write-tests
description: Generate comprehensive tests for existing code. Use when asked to "add tests", "write unit tests", "improve test coverage", or "test this function/class/module".
argument-hint: [file-path or function-name]
allowed-tools: Read, Edit, Write, Glob, Grep, Bash
---

# Write Tests for $ARGUMENTS

## Step 1: Understand the Code Under Test

Read the target file(s) and understand:
- What each function/method does
- Input types and expected outputs
- Side effects (DB writes, API calls, file I/O)
- Dependencies and how they're injected

Also read existing tests to learn the project's testing patterns, assertions, and test utilities.

## Step 2: Identify Test Cases

For each unit of code, enumerate:

| Category | What to test |
|----------|-------------|
| Happy path | Normal inputs → expected outputs |
| Edge cases | Empty, zero, null, max values |
| Error cases | Invalid input, missing deps, network failure |
| Boundaries | Off-by-one, type coercion, overflow |
| Side effects | Mocks called with right args, DB state |

## Step 3: Write the Tests

Follow the project's existing test framework and style. When in doubt:
- **Arrange**: Set up the inputs and mocks
- **Act**: Call the function/method
- **Assert**: Verify outputs and side effects

Naming convention: `test_<what>_<condition>_<expected_result>` or equivalent.

### Guidelines
- Each test should test exactly one thing
- Tests must be deterministic (no random values, no sleep)
- Mock external I/O (HTTP, database, filesystem) unless it's an integration test
- Do not test implementation details — test behavior

## Step 4: Verify Tests Pass

Run the test suite. Fix any failures in the new tests (not in the source code unless there's a genuine bug).

## Step 5: Report Coverage

After writing tests, summarize:
- Functions/methods now covered
- Any uncoverable paths and why (dead code, untestable side effects)
- Suggested follow-up: integration tests, property-based tests, etc.
