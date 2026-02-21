---
name: generate-docs
description: Generate documentation for code — docstrings, API docs, README sections, or architecture docs. Use when asked to "document this", "add docstrings", "write API docs", or "document this module".
argument-hint: [file-path or module-name]
allowed-tools: Read, Edit, Write, Glob, Grep
---

# Generate Documentation for $ARGUMENTS

## Step 1: Read the Code

Read the target file(s) thoroughly. Identify:
- Public API surface (exported functions, classes, methods)
- Parameters and return types
- Exceptions or errors that can be raised
- Side effects (mutations, I/O, network calls)
- Any existing documentation (to extend, not replace)

## Step 2: Determine Documentation Type

| Request | Output |
|---------|--------|
| "Add docstrings" | In-code docstrings for all public members |
| "API docs" | Markdown reference documentation |
| "README section" | Markdown usage section with examples |
| "Architecture docs" | High-level design document |

## Step 3: Write the Documentation

### For Docstrings

Use the language's standard format:

**Python (Google style)**:
```python
def function(param1: str, param2: int = 0) -> bool:
    """Short one-line summary.

    Longer description if needed. Explain any non-obvious behavior,
    preconditions, or postconditions.

    Args:
        param1: Description of param1.
        param2: Description of param2. Defaults to 0.

    Returns:
        True if successful, False otherwise.

    Raises:
        ValueError: If param1 is empty.
    """
```

**TypeScript/JavaScript (JSDoc)**:
```typescript
/**
 * Short one-line summary.
 *
 * Longer description if needed.
 *
 * @param param1 - Description of param1
 * @param param2 - Description of param2 (default: 0)
 * @returns True if successful, False otherwise
 * @throws {Error} If param1 is empty
 * @example
 * ```ts
 * const result = myFunction("hello", 42);
 * ```
 */
```

### For Markdown API Docs

```markdown
## `functionName(param1, param2)`

Short description.

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `param1` | `string` | Yes | Description |
| `param2` | `number` | No | Description. Default: `0` |

**Returns**: `boolean` — Description of return value.

**Example**

\`\`\`ts
const result = functionName("hello", 42);
// result: true
\`\`\`
```

### Quality Standards
- Every public function, class, and method must have documentation
- Examples must be correct and runnable
- Document exceptions and error conditions
- Keep descriptions concise — one clear sentence is better than three vague ones
- Do not describe the implementation, describe the behavior

## Step 4: Verify

After writing:
- Re-read each docstring — would someone unfamiliar with the code understand it?
- Check that examples are syntactically correct
- Ensure no public API is left undocumented
