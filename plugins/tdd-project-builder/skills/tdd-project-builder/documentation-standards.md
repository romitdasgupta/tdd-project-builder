# Documentation Standards

Reference for Phases 3, 4, and 5. Read before updating any documentation.

## Four Documentation Layers

| Layer | Scope | What to Write |
|-------|-------|---------------|
| Code | Functions | Docstrings on public functions (what, params, returns, errors). Inline comments explain *why*, not *what*. Type annotations where supported. Skip obvious code like `getName()`. |
| Module | Directories | Purpose doc per module: responsibility, dependencies, dependents. Usage examples for every exported interface. |
| Project | Repository | README.md, ARCHITECTURE.md, API.md, TESTING.md, CONTRIBUTING.md. Stubs in Phase 2, maintained every commit, verified Phase 5. |
| Tests | Specifications | BDD names are living specs. Test files mirror source structure. System behavior understandable from test names alone. |

## Pre-Commit Sync Checklist

Before every commit, check each row. Any "yes" without a doc update blocks the commit.

| Changed? | Update |
|----------|--------|
| Public interface | API.md + docstrings |
| Module responsibilities | ARCHITECTURE.md |
| Setup or dependencies | README.md + CONTRIBUTING.md |
| Test strategy | TESTING.md |
| Behavior under test | Rename tests to match |

## Quality Standard

Litmus test: could a developer who has never seen this codebase understand it without asking someone?

- No jargon without definition
- No "see X" without a link or file path
- No outdated references to refactored code
- Examples use real values, never foo/bar/placeholder

## Project Doc Templates

| Document | Must Contain |
|----------|-------------|
| README.md | What the project does, install steps, quick start, usage examples, license |
| ARCHITECTURE.md | Module map, layer diagram, data flow, dependency direction |
| API.md | Every public interface: signature, description, example |
| TESTING.md | Strategy, how to run each test type, coverage summary |
| CONTRIBUTING.md | Setup guide, code conventions, how to add a feature, how to submit |

## Phase 5 Verification Checklist

Final pass before marking docs complete:

1. **README.md** -- quick start works, install steps tested, basic usage shown
2. **ARCHITECTURE.md** -- module map matches final file structure, data flow accurate
3. **API.md** -- every public interface present, no stale references
4. **TESTING.md** -- strategy current, all test types documented, run instructions verified
5. **CONTRIBUTING.md** -- setup guide tested on clean environment, conventions match actual code
