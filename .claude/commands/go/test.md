# Run Tests

Run the test suite for the current changes or specified scope.

## Usage

```
/go/test [scope] [options]
```

## Arguments

- `scope` (optional): Specific test file, directory, or pattern to run. Defaults to full test suite.
- `options` (optional): Additional flags like `--watch`, `--coverage`, `--verbose`

## What This Does

1. **Detects test framework** — Checks `package.json` for Jest, Vitest, Mocha, or other configured test runner
2. **Runs relevant tests** — Executes tests scoped to changed files when possible
3. **Reports results** — Summarizes pass/fail counts, coverage if available, and any failures
4. **Suggests fixes** — For failing tests, analyzes the failure and suggests likely fixes

## Steps

```javascript
// 1. Read package.json to find test script and framework
const pkg = read('package.json');
const testScript = pkg.scripts?.test;
const testFramework = detectFramework(pkg);

// 2. Determine scope
const changedFiles = await getChangedFiles(); // git diff --name-only
const testFiles = changedFiles
  .filter(f => f.match(/\.(test|spec)\.(js|ts|mjs)$/))
  .concat(changedFiles.map(f => findRelatedTests(f)));

// 3. Run tests
const result = await run(`npm test -- ${scope || testFiles.join(' ')} ${options}`);

// 4. Parse and report
reportResults(result);
```

## Examples

```bash
# Run all tests
/go/test

# Run tests for a specific file
/go/test src/commands/add.js

# Run with coverage
/go/test --coverage

# Run in watch mode
/go/test --watch

# Run a specific test pattern
/go/test --grep "task creation"
```

## Output Format

After running, provide a summary like:

```
✅ Tests passed: 42
❌ Tests failed: 2
⏭️  Tests skipped: 5
📊 Coverage: 78.3%

Failed tests:
- task-manager.test.js: "should handle missing config" 
  → Expected undefined, received null (line 47)
- cli.test.js: "add command with no args"
  → Process exited with code 1, expected 0 (line 112)
```

## Notes

- If no test framework is detected, ask the user which one to use
- For large test suites, prefer running only tests related to changed files unless `--all` is passed
- Always show the full error output for failing tests, not just the summary
- If tests fail due to missing dependencies, suggest running `npm install` first
