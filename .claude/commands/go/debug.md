# Debug Command

Investigate and fix a bug or unexpected behavior in the codebase.

## Usage

```
/go/debug [issue description or error message]
```

## What This Does

This command helps you systematically debug issues by:
1. Understanding the problem
2. Gathering context and reproducing the issue
3. Identifying root cause
4. Implementing a fix
5. Verifying the fix works

## Process

### 1. Understand the Problem

First, clarify what's happening:
- What is the expected behavior?
- What is the actual behavior?
- Is there an error message or stack trace?
- When did this start happening?

If the user provided an error message or description, use that as the starting point.

### 2. Gather Context

```bash
# Check recent changes that might have introduced the bug
git log --oneline -20
git diff HEAD~5..HEAD -- [relevant files]

# Look at the failing tests if any
npm test -- --verbose 2>&1 | tail -50
```

### 3. Reproduce the Issue

Try to reproduce the bug consistently:
- Run the relevant code path
- Check logs for errors
- Inspect state at the point of failure

### 4. Identify Root Cause

Trace through the code:
- Follow the execution path
- Check for off-by-one errors, null/undefined values, async issues
- Look for recent changes in related files
- Check if dependencies changed

### 5. Implement Fix

Make the minimal change needed to fix the issue:
- Don't refactor while fixing (separate concerns)
- Add a comment explaining why the fix is needed if non-obvious
- Consider edge cases

### 6. Verify

```bash
# Run relevant tests
npm test -- --testPathPattern=[relevant test file]

# Run full test suite to check for regressions
npm test

# If it's a CLI issue, test manually
node [entry point] [args]
```

## Tips

- **Start narrow**: Focus on the specific failing behavior before broadening scope
- **Check assumptions**: Verify that inputs/outputs match what you expect at each step
- **Binary search**: If unsure where the bug is, add logging to narrow down the location
- **Read the error**: Error messages often point directly to the problem
- **Check types**: Many JS bugs come from unexpected types (undefined, null, NaN)
- **Async pitfalls**: Missing awaits and unhandled promise rejections are common culprits

## Common Patterns in This Codebase

- Task operations go through `src/core/task-manager.js`
- AI calls are wrapped in `src/ai/` modules
- CLI commands are in `src/commands/`
- Config is loaded via `src/config/`

## After Fixing

Once the fix is in place, consider:
- Should a test be added to prevent regression?
- Does the fix expose a broader design issue worth noting?
- Should this be documented somewhere?

Use `/go/commit` to commit the fix with a clear message describing what was broken and how it was fixed.
