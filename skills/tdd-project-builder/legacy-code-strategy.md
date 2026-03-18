# Legacy Code Strategy

Reference for Phase 1. Read when legacy (untested) code is detected.

## Core Principle

Never modify code you can't test. Wrap it with characterization tests first.

## Characterization Testing Process

1. **Identify** the module to be modified
2. **Capture current behavior** -- write tests documenting what the code actually does, not what it should do
3. **Run tests** to verify they pass against existing code
4. **Mark known bugs** in test names: `"should return -1 for empty input (BUG: should return 0, see issue #X)"`
5. **Proceed** -- only after all characterization tests pass, modify code using normal TDD (RED-GREEN-REFACTOR)

## Interaction with Modularity Rules

- Hard rules apply only to **new and modified** code
- Existing untested code is NOT refactored until wrapped with characterization tests
- Once a module has characterization tests, refactor it to meet modularity rules through normal RED-GREEN-REFACTOR
- Gradual adoption -- don't try to fix everything at once

## Codebase Assessment Checklist

Perform during Phase 1, document findings before Phase 2.

| Question | Result | Path |
|----------|--------|------|
| Does a test directory/framework exist? | Yes → has tests | Existing with tests: run suite, map coverage gaps |
| Any tests at all? | No → no tests | Legacy: characterization tests before any changes |
| Partial coverage? | Some modules tested | Map what's covered, treat uncovered modules as legacy |

**Output:** A written assessment listing each module's test status (tested, partially tested, untested) carried into Phase 2 as the gap list.
