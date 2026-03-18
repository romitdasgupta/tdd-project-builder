# tdd-project-builder

> Strict TDD project construction with full test pyramid, living documentation, and modular code enforcement

A Claude Code plugin that orchestrates building entire software projects through strict Test-Driven Development. It enforces a full test pyramid (unit, functional, integration, e2e), maintains living documentation alongside every code change, and gates code quality through automated review agents. Works with any language or framework.

## Installation

**Step 1** — Add the marketplace:

```
/plugin marketplace add romitdasgupta/tdd-project-builder
```

**Step 2** — Install the plugin:

```
/plugin install tdd-project-builder@tdd-project-builder
```

**Step 3** — Activate:

```
/reload-plugins
```

### Dependency

This plugin depends on the [superpowers](https://github.com/anthropics/claude-code-plugins) plugin for its TDD and verification skills. Install it first if you haven't already:

```
/plugin marketplace add anthropics/claude-code-plugins
/plugin install superpowers@claude-code-plugins
```

## Usage

The skill triggers automatically when Claude detects a project-building task -- for example:

- "Build me a REST API"
- "Add a caching layer to this app"
- "This codebase has no tests, help me add coverage"

It works with any language or framework and handles greenfield projects, feature additions, and legacy codebases.

### The 5 Phases

1. **ASSESS** -- Determine codebase state (greenfield, existing, or legacy) and inventory what exists.
2. **ARCHITECT** -- Design a modular structure with inside-out dependency layering.
3. **BUILD** -- TDD per module: write failing unit tests, implement to pass, add functional tests, refactor, pass the review gate, update docs, commit.
4. **INTEGRATE** -- Add integration and e2e tests, then verify the full test suite passes.
5. **VERIFY DOCS** -- Final documentation review pass to ensure everything is accurate and complete.

## What's Included

| File | Description |
|------|-------------|
| `SKILL.md` | Orchestrator -- 5-phase process, flowchart, and reference loading directives |
| `test-pyramid-guide.md` | Test types, BDD naming conventions, Arrange-Act-Assert pattern, isolation rules |
| `documentation-standards.md` | Doc layers, sync checklist, quality standard, Phase 5 verification |
| `modularity-rules.md` | Hard rules for module boundaries, review gate mechanism, conflict resolution |
| `legacy-code-strategy.md` | Characterization testing, wrap-before-modify pattern, gradual adoption path |

All reference files live in `plugins/tdd-project-builder/skills/tdd-project-builder/`.

## Philosophy

- **Strict TDD** -- No production code is written without a failing test first.
- **Full test pyramid** -- Unit, functional, integration, and e2e tests for every project.
- **Living documentation** -- Updated in every commit, never treated as an afterthought.
- **Modular code** -- Enforced by automated review gates with hard limits on coupling and complexity.

## License

See [LICENSE](LICENSE) for details.
