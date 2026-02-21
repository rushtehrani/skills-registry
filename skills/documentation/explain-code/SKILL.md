---
name: explain-code
description: Explain how code works using analogies, diagrams, and step-by-step walkthroughs. Use when asked "how does this work?", "explain this code", "walk me through this", or "what does this do?".
argument-hint: [file-path or function-name]
allowed-tools: Read, Glob, Grep
---

# Explain Code: $ARGUMENTS

## Step 1: Read and Understand

Read the target code thoroughly before explaining anything. Also read:
- Any interfaces or types it uses
- Its callers (how it's used in practice)
- Its dependencies (what it calls)

## Step 2: Structure the Explanation

Always explain in this order:

### The Analogy
Start with a real-world analogy that captures the essence. Example:
- A queue is like a line at a coffee shop — first in, first out
- A mutex is like a bathroom key at a restaurant — only one person holds it at a time

Keep the analogy short (1-3 sentences) and intuitive.

### The Big Picture
Describe what this code does at a high level in plain English. No jargon. One paragraph max.

### The Diagram
Draw an ASCII diagram showing:
- Data flow (arrows showing inputs → processing → outputs)
- Component relationships (boxes and connections)
- State transitions (if applicable)

Example:
```
User Request
     │
     ▼
┌─────────────┐     cache hit    ┌───────────┐
│   Handler   │ ──────────────► │   Cache   │
│             │                  └───────────┘
│             │ cache miss            │
└──────┬──────┘                       │
       │                              ▼
       │                       ┌───────────┐
       └──────────────────────►│ Database  │
                                └───────────┘
```

### Step-by-Step Walkthrough
Walk through the code line-by-line or block-by-block:
- What each section does
- Why it's written that way
- Non-obvious design choices

### The Gotcha
End with the most common misconception or surprising behavior. What trips people up? What would have bitten you if you hadn't read the code carefully?

---

## Tone Guidelines
- Write for someone unfamiliar with this specific codebase but competent in the language
- Prefer concrete over abstract
- Use the actual variable and function names from the code
- Never say "simply" or "just" — if it were simple, they wouldn't be asking
