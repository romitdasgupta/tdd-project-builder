# Test Pyramid Guide

Reference for all test phases. Read before writing any tests.

## Test Types

| Type | Scope | Written During | Focus |
|------|-------|----------------|-------|
| Unit | Single function/method | Phase 3 step 1 | Happy path, edge cases, errors, boundaries |
| Functional | One module's behavior | Phase 3 step 3 | State transitions, input validation, error handling |
| Integration | Boundaries between modules | Phase 4 step 1 | Data flow, contract adherence, error propagation |
| E2E | Complete user workflow | Phase 4 step 2 | Critical paths, failure recovery, user experience |

## Adaptation

All four types apply to every project, including pure libraries. The distinction is scope, not technology. For a pure library: unit tests cover individual functions, functional tests cover module-level behavior (e.g., a parser handling malformed input), integration tests verify modules compose correctly (e.g., a pipeline of transformations), and e2e tests exercise the public API end-to-end as a consumer would use it. For a web service, e2e means HTTP requests through the full stack. For a CLI, e2e means invoking the binary with real arguments. Adjust the mechanism, never skip the level.

## BDD Naming

Format: `"should [expected behavior] when [condition]"`

Examples:
- `"should reject expired tokens when user is not admin"`
- `"should calculate tax correctly for zero-rated items"`
- `"should return 404 when resource does not exist"`
- `"should retry failed requests when network recovers within timeout"`

Test names must be readable as specifications. A reader should understand system behavior from test names alone, without reading test bodies.

## Arrange-Act-Assert

Every test follows this structure:

```
Arrange: set up preconditions (inputs, state, dependencies)
Act:     execute the behavior under test (one call)
Assert:  verify the expected outcome (return value, state change, side effect)
```

One Act per test. Multiple asserts are fine when verifying facets of a single outcome.

## Isolation Rules

- **Unit:** Zero external dependencies. No network, no filesystem, no database.
- **Functional:** May use in-memory substitutes (in-memory DB, fake filesystem) to test module behavior.
- **Integration:** Real dependencies where feasible. Actual database connections, real HTTP calls.
- **E2E:** Full system as user would experience. No mocks, no substitutes.

## What to Test at Each Level

**Unit tests:**
- Happy path for every public function
- Edge cases (empty input, max values, unicode, nil/null)
- Error cases (invalid input, missing data, permission denied)
- Boundary values (off-by-one, type limits, empty collections)
- No mocks unless isolating from external systems

**Functional tests:**
- State transitions (created-to-active, pending-to-complete)
- Input validation as experienced by the caller
- Error handling: correct error types, messages, and recovery behavior

**Integration tests:**
- Data flow between modules (output of A is valid input to B)
- Contract adherence (interfaces match implementations)
- Error propagation across layers (low-level error surfaces correctly to caller)

**E2E tests:**
- Real user scenarios from start to finish
- Critical paths (signup, checkout, deploy)
- Failure recovery from user perspective (retry, rollback, graceful degradation)
