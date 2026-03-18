# Usage Guide

## How It Works

The `tdd-project-builder` plugin adds a skill that Claude Code activates automatically when it detects you're building a project, adding a major feature, or wrapping legacy code with tests. You can also invoke it explicitly.

### Automatic Activation

Claude will use the skill when you describe project-level work:

```
> Build me a REST API for managing tasks with authentication
> Add a caching layer to this application
> This codebase has no tests — help me add coverage
> Create a CLI tool that parses CSV files and generates reports
```

### Explicit Invocation

Use the slash command directly:

```
/tdd-project-builder:tdd-project-builder
```

## Examples

### Example 1: Greenfield Project

**You say:**
```
Build a Node.js REST API for a bookstore with CRUD operations and search
```

**What happens:**

1. **ASSESS** — Claude recognizes this is a greenfield project (empty directory) and moves straight to architecture.

2. **ARCHITECT** — Claude designs an inside-out module structure:
   - Core: `Book` model, validation logic (zero dependencies)
   - Service: `BookService` with business rules
   - Interface: Express routes (`GET /books`, `POST /books`, etc.)
   - Infrastructure: database adapter
   - Creates doc stubs: `README.md`, `ARCHITECTURE.md`, `API.md`, `TESTING.md`, `CONTRIBUTING.md`

3. **BUILD** — For each module (starting from Core):
   - Writes failing unit tests first (red)
   - Implements code until tests pass (green)
   - Adds functional tests for module behavior
   - Refactors
   - Runs a review gate (checks coupling, complexity, module boundaries)
   - Updates documentation
   - Commits code + docs together

4. **INTEGRATE** — Writes integration tests across module boundaries and end-to-end tests for complete user workflows. Runs the full test suite.

5. **VERIFY DOCS** — Final pass to ensure all documentation is accurate and a new developer could understand the repo in 30 minutes.

---

### Example 2: Adding a Feature to an Existing Codebase

**You say:**
```
Add user authentication with JWT tokens to this Express app
```

**What happens:**

1. **ASSESS** — Claude examines the existing codebase, runs existing tests, and identifies what's already there. Maps any coverage gaps.

2. **ARCHITECT** — Plans the auth modules:
   - Core: token generation/validation logic, password hashing
   - Service: `AuthService` (login, register, token refresh)
   - Interface: auth middleware, `/auth/*` routes
   - Identifies which existing modules need modification

3. **BUILD** — TDD per module. Existing tests continue to pass at every step. New modules get full unit + functional test coverage.

4. **INTEGRATE** — Integration tests verify auth works with existing routes. E2e tests cover login-then-access workflows.

5. **VERIFY DOCS** — Updates `API.md` with new endpoints, `ARCHITECTURE.md` with auth module, `README.md` with setup instructions.

---

### Example 3: Legacy Codebase with No Tests

**You say:**
```
This codebase has no tests — help me add coverage and modernize it
```

**What happens:**

1. **ASSESS** — Claude detects a legacy codebase (existing code, no tests). Loads `legacy-code-strategy.md` and writes **characterization tests** — tests that capture the current behavior without changing any code.

2. **ARCHITECT** — Identifies module boundaries in the existing code. Plans a gradual adoption path using the wrap-before-modify pattern: wrap untestable code behind interfaces before touching it.

3. **BUILD** — For each module:
   - Adds characterization tests to lock in current behavior
   - Wraps the module behind a clean interface
   - Writes unit tests for the new interface
   - Refactors the internals (characterization tests catch regressions)
   - Review gate verifies quality

4. **INTEGRATE** — Integration tests verify modules still work together after refactoring.

5. **VERIFY DOCS** — Documents the architecture, testing strategy, and how to continue the modernization.

---

### Example 4: CLI Tool

**You say:**
```
Build a Python CLI that converts markdown files to HTML with custom templates
```

**What happens:**

1. **ASSESS** — Greenfield, moves to architecture.

2. **ARCHITECT** — Inside-out design:
   - Core: markdown parser, template engine (pure functions, no I/O)
   - Service: conversion pipeline, template resolution
   - Interface: CLI argument parsing (click/argparse)
   - Infrastructure: file I/O adapters

3. **BUILD** — TDD per module. The core parser gets thorough unit tests for edge cases (nested lists, code blocks, etc.). The CLI interface gets functional tests verifying argument handling.

4. **INTEGRATE** — E2e tests run the CLI against sample markdown files and verify HTML output matches expected results.

5. **VERIFY DOCS** — `README.md` includes install instructions, usage examples, and template customization guide.

## Session Continuity

If a session is interrupted, the plugin picks up where it left off. On reload it checks:

- Git log for completed commits
- Test suite state
- Documentation completeness
- Uncommitted changes

It resumes at the earliest incomplete phase or sub-step — even mid-module in the BUILD phase.

## The Test Pyramid

Every project gets four layers of tests:

| Layer | Scope | Purpose |
|-------|-------|---------|
| **Unit** | Single function/class | Verify isolated logic |
| **Functional** | Single module | Verify module behavior as a whole |
| **Integration** | Module boundaries | Verify modules work together |
| **E2e** | Complete workflows | Verify the system works end-to-end |

## Review Gates

After each module is built, an automated review gate checks:

- Module boundaries are respected (no circular dependencies)
- Coupling stays within limits
- Complexity doesn't exceed thresholds
- Public interfaces are documented

If the gate fails, Claude fixes the violations and resubmits. Code doesn't proceed until the gate passes.

## The Golden Rule

> Every code change triggers a documentation update in the same commit. No exceptions.

Documentation is never an afterthought — it's updated alongside every piece of code, so it stays accurate throughout the project's life.
