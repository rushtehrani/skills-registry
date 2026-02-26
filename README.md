# Skills Registry

A curated registry of reusable [Claude Code](https://claude.ai/code) skills for software development teams and agents. Skills are extensions that teach Claude how to perform specific tasks — from fixing GitHub issues to auditing code for security vulnerabilities.

Skills follow the open [Agent Skills](https://agentskills.io) standard.

---

## What's a Skill?

A skill is a directory containing a `SKILL.md` file with YAML frontmatter and markdown instructions. When Claude encounters a situation matching a skill's description, it loads and applies the skill's instructions.

```
my-skill/
└── SKILL.md          # Required: frontmatter + instructions
    reference.md      # Optional: detailed reference material
    examples.md       # Optional: usage examples
    scripts/          # Optional: helper scripts
```

Skills can be invoked automatically by Claude, manually by the user (`/skill-name`), or both.

---

## Available Skills

### Development

| Skill | Description | Invocation | Pattern |
|-------|-------------|------------|---------|
| [fix-issue](skills/development/fix-issue/SKILL.md) | Fix a GitHub issue end-to-end | User only | Task |
| [code-review](skills/development/code-review/SKILL.md) | Review code for correctness, security, performance | Auto + User | Task |
| [write-tests](skills/development/write-tests/SKILL.md) | Generate comprehensive tests | Auto + User | Task |

### Git Workflows

| Skill | Description | Invocation | Pattern |
|-------|-------------|------------|---------|
| [commit](skills/git/commit/SKILL.md) | Create conventional commits | User only | Task |
| [pr-summary](skills/git/pr-summary/SKILL.md) | Summarize pull requests | Auto + User | Isolation |

### Documentation

| Skill | Description | Invocation | Pattern |
|-------|-------------|------------|---------|
| [explain-code](skills/documentation/explain-code/SKILL.md) | Explain code with analogies and diagrams | Auto + User | Reference |
| [generate-docs](skills/documentation/generate-docs/SKILL.md) | Generate docstrings and API docs | Auto + User | Task |

### Utilities

| Skill | Description | Invocation | Pattern |
|-------|-------------|------------|---------|
| [debug](skills/utilities/debug/SKILL.md) | Systematic debugging with root cause analysis | Auto + User | Task |
| [refactor](skills/utilities/refactor/SKILL.md) | Refactor code without changing behavior | Auto + User | Task |
| [security-audit](skills/utilities/security-audit/SKILL.md) | Audit for OWASP vulnerabilities and CVEs | Auto + User | Isolation |

---

## Using Skills

### Option 1: Copy to Your Project

Copy any skill directory into your project's `.claude/skills/` folder:

```bash
# Clone this registry
git clone https://github.com/rushtehrani/skills-registry.git

# Copy a skill to your project
cp -r skills-registry/skills/development/fix-issue your-project/.claude/skills/
```

Claude Code automatically discovers skills in `.claude/skills/`.

### Option 2: Copy to Personal Skills

To use skills across all your projects, copy them to your personal Claude skills directory:

```bash
cp -r skills-registry/skills/development/fix-issue ~/.claude/skills/
```

### Option 3: Reference by Path

Skills can also be referenced from any location Claude can read. Point to skills from your project's `.claude/settings.json`.

### Skill Scopes

| Scope | Location | Applies to |
|-------|----------|------------|
| Personal | `~/.claude/skills/<skill>/SKILL.md` | All your projects |
| Project | `.claude/skills/<skill>/SKILL.md` | This project only |
| Enterprise | Server-managed | All org users |

**Priority order**: Enterprise > Personal > Project

---

## Invoking Skills

**Automatic**: Claude reads skill descriptions and applies skills when they match the context. No user action required.

**Manual**: Type `/skill-name` in Claude Code to invoke directly:

```
/fix-issue 123
/code-review 456
/security-audit src/api/
/commit
```

**With arguments**: Some skills accept arguments passed via `$ARGUMENTS`:

```
/fix-issue 123          # $ARGUMENTS = "123"
/refactor src/utils.ts  # $ARGUMENTS = "src/utils.ts"
```

---

## Skill Patterns

Skills in this registry follow three patterns from the [Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf):

### Reference Skills
Always-on knowledge that Claude applies automatically. Best for coding standards, style guides, domain knowledge, and architectural conventions.

```yaml
---
name: my-standards
description: Apply our TypeScript coding standards when writing or reviewing TS code
user-invocable: false  # background knowledge only
---
```

### Task Skills
Step-by-step workflows for repeatable actions. Best for deployments, issue fixes, PR creation, and code generation.

```yaml
---
name: deploy
description: Deploy the application to production
disable-model-invocation: true  # user controls when to deploy
allowed-tools: Bash(kubectl *), Bash(docker *)
---
```

### Isolation Skills
Run in a forked subagent with focused context. Best for research, analysis, and report generation that shouldn't pollute the main conversation.

```yaml
---
name: analyze-deps
description: Analyze project dependencies and generate a report
context: fork
agent: Explore
---
```

---

## Creating a New Skill

### 1. Choose a Template

```bash
# Reference skill (always-on knowledge)
cp -r templates/reference-skill .claude/skills/my-skill

# Task skill (step-by-step workflow)
cp -r templates/task-skill .claude/skills/my-skill

# Isolation skill (runs in subagent)
cp -r templates/isolation-skill .claude/skills/my-skill
```

### 2. Edit SKILL.md

Open `.claude/skills/my-skill/SKILL.md` and:
1. Set `name` to your skill's directory name
2. Write a `description` with keywords that match natural language triggers
3. Replace the template content with your instructions

### 3. Configure Frontmatter

Key options:

```yaml
---
name: my-skill
description: When and why to use this skill
argument-hint: [what-the-argument-means]   # autocomplete hint
disable-model-invocation: true             # user-only invocation
user-invocable: false                      # Claude-only (hidden from menu)
allowed-tools: Read, Grep, Bash(git *)    # restrict tool usage
context: fork                              # run in isolated subagent
agent: Explore                             # subagent type (with context: fork)
model: claude-opus-4-6                    # specific model for this skill
---
```

### 4. Test Your Skill

```
/my-skill [arguments]
```

Verify Claude follows the instructions correctly. Iterate on the description if auto-invocation triggers too often or not enough.

### 5. Contribute

To add your skill to this registry:
1. Place it in the appropriate category under `skills/`
2. Add an entry to `registry.json`
3. Open a pull request

---

## Frontmatter Reference

| Key | Type | Description |
|-----|------|-------------|
| `name` | string | Display name (defaults to directory name) |
| `description` | string | When Claude should invoke this skill |
| `argument-hint` | string | Autocomplete hint for expected arguments |
| `disable-model-invocation` | boolean | Only user can invoke |
| `user-invocable` | boolean | Set `false` to hide from user menu |
| `allowed-tools` | string | Comma-separated list of permitted tools |
| `model` | string | Specific model to use for this skill |
| `context` | `fork` | Run in isolated subagent |
| `agent` | string | Subagent type: `Explore`, `Plan`, `general-purpose` |

### String Substitutions

| Placeholder | Replaced with |
|-------------|--------------|
| `$ARGUMENTS` | All arguments passed to the skill |
| `$1`, `$2`, ... | Specific argument by position |
| `${CLAUDE_SESSION_ID}` | Current session ID |

### Dynamic Context with Shell Injection

Use `` !`command` `` in skill content to inject live data:

```markdown
Current branch: !`git rev-parse --abbrev-ref HEAD`
Open PRs: !`gh pr list --json number,title`
```

---

## Registry Index

The machine-readable skill index is at [`registry.json`](registry.json). It lists all skills with their metadata for programmatic discovery.

---

## License

MIT — see [LICENSE](LICENSE).
