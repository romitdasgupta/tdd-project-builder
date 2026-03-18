# Modularity Rules

Reference for Phase 3 step 5. Read before running the review gate.

## Hard Rules

| Rule | Limit | Counting Method |
|------|-------|-----------------|
| Function length | Max 20 lines | Lines of logic -- excludes blank lines, comments, signature |
| File length | Max 200 lines | All lines including imports, comments, blanks |
| Function parameters | Max 4 | Each parameter; destructured objects count as 1 |
| Nesting depth | Max 3 levels | From function body; if/for/while/try each add a level |
| Single responsibility | 1 purpose per module | Describable in one sentence without "and" |
| Dependency direction | Inward only | Outer layers depend on inner, never reverse |
| Dead code | Zero tolerance | No commented-out code, no unused functions |
| Naming | Self-documenting | Reader understands purpose from name alone |

## Scope of Enforcement

Rules apply to new and modified code only. Pre-existing code doesn't need to conform until touched. When modifying an existing file, fix violations in the code you touch -- don't refactor the entire file unless it has test coverage.

## Review Gate

Dispatch a subagent via Claude Code `Agent` tool with the module's source and test file paths, these modularity rules as inline context, and instructions to return:

```
VERDICT: PASS or FAIL
VIOLATIONS (if FAIL):
- [ ] Rule: <rule name> | File: <path> | Line: <number> | Detail: <what's wrong>
NOTES (if any):
- <qualitative observations>
```

## Review Checklist

**Pass/fail checks (any failure blocks):**
1. All hard rules in the table above
2. Every public function has a docstring
3. Test names are BDD-style and readable
4. No unresolved TODO/FIXME/HACK comments

**Qualitative checks (agent judgment):**
1. Could a stranger understand this module in under 5 minutes?
2. Are names descriptive and consistent?
3. Is there repeated logic that should be extracted?
4. Does the module depend only on what it needs?

## Conflict Resolution

Priority when rules conflict:
1. **Readability** -- always wins
2. **Single responsibility** -- one thing done well
3. **Size limits** -- serve readability, not the other way around

A 2-3 line exception over a limit is acceptable if more readable, but must be explicitly noted and justified.
