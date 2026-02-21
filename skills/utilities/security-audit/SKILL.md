---
name: security-audit
description: Audit code or a repository for security vulnerabilities. Use when asked to "audit for security", "find security issues", "check for vulnerabilities", or "is this secure?".
argument-hint: [file-path or module-name]
context: fork
agent: Explore
allowed-tools: Read, Glob, Grep, Bash(npm *), Bash(pip-audit*), Bash(gh *)
---

# Security Audit: $ARGUMENTS

## Scope

Audit the specified code or the entire repository if no target is given. Check for both code-level vulnerabilities and dependency-level vulnerabilities.

---

## Audit Categories

### 1. Injection Vulnerabilities
- **SQL injection**: String concatenation in queries — require parameterized queries
- **Command injection**: User input passed to shell commands
- **XSS**: Unescaped user content rendered as HTML
- **Path traversal**: User-controlled file paths without validation
- **SSRF**: User-controlled URLs in server-side HTTP requests

### 2. Authentication & Authorization
- Missing authentication on sensitive routes
- Broken access control (users accessing other users' data)
- Insecure session management
- JWT without signature verification
- Missing rate limiting on login/auth endpoints

### 3. Secrets & Credentials
- Hardcoded API keys, passwords, tokens in source code
- Secrets in environment variable names that may be logged
- Credentials committed to version control
- Overly permissive API keys (write access when read-only suffices)

### 4. Cryptography
- Weak hashing algorithms (MD5, SHA1 for passwords — use bcrypt/argon2)
- Insecure random number generation (Math.random() for tokens)
- Broken encryption (ECB mode, short keys, reused IVs)
- Sensitive data stored in plaintext

### 5. Dependency Vulnerabilities

Run dependency audit tools:
```bash
# Node.js
npm audit

# Python
pip-audit

# Or check known CVE databases for listed dependencies
```

### 6. Insecure Defaults
- Debug mode enabled in production
- Overly permissive CORS (`*` origin)
- Missing security headers (CSP, HSTS, X-Frame-Options)
- Verbose error messages exposing stack traces to users

### 7. Data Exposure
- Sensitive fields returned in API responses
- Logging of passwords, tokens, or PII
- Insecure storage of sensitive data (localStorage for tokens, etc.)

---

## Output Format

**Executive Summary**: Overall risk level (Critical / High / Medium / Low) and top 3 concerns.

**Findings**:

For each issue found:
```
[SEVERITY] Category: Short title
File: path/to/file.ts:line
Description: What the vulnerability is and how it could be exploited
Recommendation: Specific code change to fix it
```

Severity levels:
- **Critical**: Immediate exploitation possible, data loss or RCE risk
- **High**: Significant risk, should fix before next release
- **Medium**: Real risk but requires specific conditions
- **Low**: Defense-in-depth improvements

**Dependency Vulnerabilities**: List any CVEs found with CVSS score and fix version.

**Not Checked**: List areas outside the audit scope (e.g., infrastructure, network config).
