---
# TEMPLATE: Isolation Skill
#
# Isolation skills run in a separate subagent (forked context).
# Use this pattern for: research tasks, exploration, analysis, report generation,
# or any work that should run independently without the full conversation history.
#
# Benefits of isolation:
# - Focused context: subagent only receives this skill's instructions
# - Parallel execution: multiple isolation skills can run concurrently
# - Clean output: result is returned to main conversation as a summary

name: my-isolation-skill
description: |
  Describe the isolated task. Example: "Analyze the codebase architecture
  and generate a dependency graph. Use when asked to visualize project
  structure, understand module relationships, or map the codebase."
argument-hint: [optional-argument]

# Required for isolation:
context: fork

# Choose the most appropriate subagent type:
# - Explore: for research, file reading, codebase analysis
# - Plan: for designing solutions and architectures
# - general-purpose: for tasks requiring code execution and file writing
agent: Explore

# Restrict tools to what this isolated task actually needs:
allowed-tools: Read, Glob, Grep, Bash(git log), Bash(git diff)
---

# [Isolated Task Name]

<!-- The content below becomes the full task prompt for the subagent.
     The subagent does NOT have access to the conversation history.
     Be explicit and self-contained. -->

## Context

<!-- Provide any context the subagent needs to understand its task.
     Use shell command injection to pull in live data: -->

Current branch: !`git rev-parse --abbrev-ref HEAD`
Recent commits: !`git log --oneline -10`

<!-- The !`command` syntax executes the command and injects its output
     before sending the prompt to the subagent. -->

---

## Your Task

<!-- Describe exactly what the subagent should do. -->

1. [First thing to investigate or do]
2. [Second thing]
3. [Third thing]

---

## Output Format

<!-- Specify exactly what you want returned. The subagent's final
     response is returned to the main conversation. -->

Return your findings as:

**Summary**: One paragraph.

**Details**:
- Finding 1
- Finding 2

**Recommendations**:
- Action 1
- Action 2

<!-- Tips for writing good isolation skills:
     - Be explicit: the subagent has no conversation history
     - Use !`command` to inject dynamic context (file lists, git state, etc.)
     - Specify output format precisely — that's what comes back to the user
     - Use agent: Explore for read-only research tasks
     - Use agent: general-purpose when the subagent needs to write files
     - Keep the scope narrow — isolation works best for focused tasks -->
