# Skills Registry

This repository contains Claude Code skills — reusable prompt templates that extend Claude Code with custom slash commands.

## What are Skills?

Skills are markdown files that teach Claude how to perform a specific, well-defined task. When a skill is installed, it becomes available as a slash command (e.g., `/session-start-hook`). Claude reads the skill's instructions at runtime and follows them to complete the task.

## Repository Structure

Each skill lives in its own directory:

```
<skill-name>/
└── SKILL.md
```

## SKILL.md Format

Every `SKILL.md` file must begin with YAML frontmatter followed by the skill's instructions:

```markdown
---
name: your-skill-name
description: One or two sentences describing what the skill does and when Claude should use it.
---

# Skill Title

Instructions for Claude go here...
```

**Frontmatter fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | The skill identifier (kebab-case) |
| `description` | Yes | Shown to Claude to determine when to invoke this skill. Be specific about the use case. |

## How to Create a Skill

### 1. Create the skill directory and file

```bash
mkdir <skill-name>
touch <skill-name>/SKILL.md
```

### 2. Write the frontmatter

```markdown
---
name: my-skill
description: Describe what this skill does and when to use it. Include trigger conditions so Claude knows when to invoke it automatically.
---
```

### 3. Write the skill instructions

The body of `SKILL.md` is a prompt Claude will follow. Structure it clearly:

- **Start with a one-line summary** of what the skill accomplishes
- **Break work into numbered steps** — Claude works best with explicit, sequential workflows
- **Include code templates** for any files or scripts the skill creates
- **Specify validation steps** so Claude can verify success
- **End with a wrap-up section** describing what to report back to the user

### 4. Add it to this registry

Open a pull request with your new `<skill-name>/SKILL.md` file.

## Writing Effective Skills

**Be explicit about workflows.** Use a numbered list of steps and tell Claude to make a todo list for them:

```markdown
## Workflow

Make a todo list for all the tasks in this workflow and work on them one after another.

### 1. Analyze the project
...

### 2. Create the output
...

### 3. Validate
...
```

**Include concrete examples.** Show exact file contents, command invocations, and expected output:

````markdown
### Create the config file

```bash
cat > .claude/settings.json << 'EOF'
{
  "hooks": {}
}
EOF
```
````

**Describe the wrap-up.** Tell Claude exactly what summary to provide when done:

```markdown
## Wrap up

Provide a summary with:
* What was created or changed
* Validation results (✅ success or ‼️ failure with details)
* Any follow-up steps the user should take
```

**Scope the description carefully.** The `description` field is used by Claude to decide when to invoke a skill automatically. Make it specific enough to avoid false positives:

```yaml
# Too broad:
description: Helps with hooks.

# Better:
description: Creates a SessionStart hook for Claude Code on the web. Use when the user wants to set up a repository so that dependencies are installed at the start of each session.
```

## Installing a Skill Locally

To use a skill from this registry in your Claude Code sessions:

```bash
# Clone this registry
git clone <registry-url> skills-registry

# Copy the skill to your Claude skills directory
cp -r skills-registry/<skill-name> ~/.claude/skills/
```

Claude Code automatically detects skills in `~/.claude/skills/` and makes them available as slash commands.

## Example Skill

See [`session-start-hook/SKILL.md`](session-start-hook/SKILL.md) for a complete, real-world example demonstrating:

- Proper frontmatter
- Multi-step workflow structure
- Inline code templates
- Validation steps
- User-facing wrap-up summary
