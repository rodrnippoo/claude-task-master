# Test Coverage Analysis

Analyze and improve test coverage for the codebase or specific modules.

## Usage

```
/go/test-coverage [target] [--threshold=80] [--fix]
```

## Arguments

- `target` (optional): Specific file, directory, or module to analyze. Defaults to entire project.
- `--threshold=N`: Minimum acceptable coverage percentage (default: 80)
- `--fix`: Automatically generate missing tests to meet threshold

## What This Does

1. **Runs coverage analysis** using the project's test runner with coverage flags
2. **Identifies uncovered code** — functions, branches, and lines missing tests
3. **Prioritizes gaps** by criticality (core logic > utilities > config)
4. **Generates missing tests** if `--fix` is passed
5. **Reports summary** with per-file breakdown

## Steps

### 1. Detect test runner and coverage tool

Check `package.json` for test scripts and coverage configuration:
- Jest: `jest --coverage`
- Vitest: `vitest run --coverage`
- c8/nyc: wraps existing test command

### 2. Run coverage

```bash
npm run test:coverage
# or fallback
npx jest --coverage --coverageReporters=json-summary,text
```

### 3. Parse results

Read from `coverage/coverage-summary.json` or stdout. Extract:
- Overall: statements, branches, functions, lines %
- Per-file breakdown sorted by lowest coverage first

### 4. Identify critical gaps

Focus on files that:
- Have 0% coverage and are imported by many other files
- Contain exported functions with no tests
- Have complex branching logic (cyclomatic complexity > 5) with low branch coverage

### 5. Generate tests (if --fix)

For each uncovered function:
- Read the source file
- Understand the function signature, inputs, outputs, and side effects
- Write meaningful tests — not just "it exists" checks
- Cover happy path, edge cases, and error conditions
- Place tests in the appropriate `__tests__/` directory or `.test.js` sibling file

### 6. Verify improvement

Re-run coverage after generating tests to confirm threshold is met.

## Output

```
Coverage Report
===============
Statements : 74.3% (threshold: 80%) ❌
Branches   : 61.2% (threshold: 80%) ❌  
Functions  : 82.1% (threshold: 80%) ✅
Lines      : 75.0% (threshold: 80%) ❌

Lowest coverage files:
  src/core/task-manager.js     — 42% (12 uncovered functions)
  src/utils/dependency-graph.js — 55% (8 uncovered branches)
  src/commands/expand.js        — 61% (5 uncovered lines)

Generated 23 new tests across 3 files.
New coverage: 83.1% ✅
```

## Notes

- Don't chase 100% coverage blindly — focus on meaningful tests
- Generated tests should assert behavior, not just call functions
- If a function is genuinely untestable (e.g., CLI entry point), add to coverage ignore list
- Check for existing `.nycrc`, `jest.config.js`, or `vitest.config.js` before assuming defaults
