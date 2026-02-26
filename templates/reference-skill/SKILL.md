---
# TEMPLATE: Reference Skill
#
# Reference skills provide always-on knowledge that Claude applies automatically.
# Use this pattern for: coding standards, style guides, domain knowledge,
# architectural conventions, team preferences.
#
# Claude loads skill descriptions into context at all times.
# Full skill content loads only when the skill is invoked.

name: my-reference-skill
description: |
  Describe WHEN Claude should apply this knowledge. Use specific keywords
  that match natural language. Example: "Apply our TypeScript coding standards
  whenever writing or reviewing TypeScript code. Includes naming conventions,
  error handling patterns, and module organization rules."

# Optional: prevent users from invoking manually (background knowledge only)
# user-invocable: false

# Optional: restrict tools Claude uses when this skill is active
# allowed-tools: Read, Grep, Glob
---

# [Skill Name]: Reference Guide

<!-- One-paragraph description of what this skill covers and when it applies. -->

---

## [Topic 1]

Describe the convention, rule, or pattern. Be specific and actionable.

**Do:**
```
# Concrete example of correct usage
```

**Don't:**
```
# Concrete example of what to avoid
```

**Why:** Briefly explain the reasoning behind this rule.

---

## [Topic 2]

<!-- Repeat the structure above for each major topic. -->

---

## [Topic 3]

<!-- Keep each section focused. If a section grows beyond ~20 lines,
     move it to a separate reference.md file and link to it here. -->

---

## Additional Reference

For detailed examples, see [examples.md](./examples.md).

<!-- Tips for writing good reference skills:
     - Write in present tense: "Functions return..." not "Functions should return..."
     - Use concrete code examples, not abstract descriptions
     - Keep the total file under 500 lines; use supporting files for overflow
     - Descriptions should contain keywords that trigger this skill automatically
     - Test by asking Claude questions where this knowledge is relevant -->
