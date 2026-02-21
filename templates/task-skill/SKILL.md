---
# TEMPLATE: Task Skill
#
# Task skills guide Claude through multi-step workflows.
# Use this pattern for: deployments, code generation, issue fixes,
# PR creation, migrations, and other repeatable action sequences.
#
# Key decision: should Claude invoke this automatically, or only when
# explicitly requested by the user?
# - For side-effect actions (deploy, commit, delete): use disable-model-invocation: true
# - For analytical tasks Claude can run on its own: leave it enabled

name: my-task-skill
description: |
  Describe the task and when to invoke it. Include action keywords.
  Example: "Deploy the application to production. Use when asked to deploy,
  ship, release, or push to production."
argument-hint: [argument-description]

# Recommended for tasks with side effects:
disable-model-invocation: true

# Restrict to only the tools this task needs:
allowed-tools: Read, Edit, Write, Bash(git *), Bash(gh *)
---

# [Task Name]: $ARGUMENTS

<!-- Replace $ARGUMENTS with a description of what the argument represents,
     or remove if the task takes no arguments. -->

## Prerequisites

<!-- List any conditions that must be true before starting.
     Claude should verify these before proceeding. -->
- [ ] Precondition 1
- [ ] Precondition 2

---

## Step 1: [First Action]

<!-- Describe what to do. Be specific. -->

Commands to run (if applicable):
```bash
some-command --with-flags
```

What to verify after this step:
- Expected output or state

---

## Step 2: [Second Action]

<!-- Continue pattern for each step. -->

---

## Step 3: [Third Action]

---

## Completion Criteria

The task is complete when:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| `Error message` | What causes it | How to fix it |

<!-- Tips for writing good task skills:
     - Use numbered steps so Claude can track progress
     - Include verification after each consequential step
     - Add disable-model-invocation: true for destructive or irreversible actions
     - Restrict allowed-tools to exactly what the task needs
     - Include error handling for the most common failure modes
     - Use $ARGUMENTS, $1, $2, etc. for parameterized tasks -->
