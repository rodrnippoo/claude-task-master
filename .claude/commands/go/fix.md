# Fix Issues Command

Automatically diagnose and fix issues in the codebase based on error output, failing tests, or lint warnings.

## Usage

```
/go/fix [issue description or error message]
```

## What This Does

1. **Identify the problem** — Parse error messages, stack traces, or descriptions to understand what's broken
2. **Locate the source** — Find the relevant files, functions, or lines causing the issue
3. **Apply the fix** — Make targeted, minimal changes to resolve the issue
4. **Verify the fix** — Run tests or checks to confirm the issue is resolved
5. **Summarize changes** — Provide a clear explanation of what was changed and why

## Steps

### 1. Understand the Issue

If an error message or description is provided, analyze it carefully:
- Identify error type (syntax, runtime, logic, type, lint)
- Extract file paths and line numbers from stack traces
- Note any relevant context (environment, recent changes)

If no specific issue is provided:
- Run `npm test` or `npm run lint` to discover issues
- Check recent git changes for potential regressions: `git diff HEAD~1`

### 2. Investigate Root Cause

Before making any changes:
- Read the failing file(s) in full context
- Check related files that might be involved
- Look at recent commits that touched these files: `git log --oneline -10 -- <file>`
- Understand the intended behavior vs actual behavior

### 3. Apply Minimal Fix

- Make the smallest change that resolves the issue
- Avoid refactoring unrelated code
- Preserve existing code style and conventions
- Add comments if the fix is non-obvious

### 4. Verify

After applying the fix:
- Run relevant tests: `npm test -- --testPathPattern=<affected-area>`
- Run lint if applicable: `npm run lint`
- Manually trace through the logic if automated checks aren't available

### 5. Report

Provide a summary:
- **Problem**: What was broken and why
- **Fix**: What was changed
- **Files modified**: List of changed files
- **Verification**: How the fix was confirmed

## Examples

```
/go/fix TypeError: Cannot read property 'id' of undefined at tasks.js:142
```

```
/go/fix tests are failing in the sync module
```

```
/go/fix
```
(will auto-detect issues by running tests/lint)

## Notes

- If multiple issues are found, fix them one at a time and verify each
- If a fix requires significant refactoring, stop and describe the approach before proceeding
- For issues that are unclear or ambiguous, ask for clarification rather than guessing
- Always prefer fixing the root cause over patching symptoms
