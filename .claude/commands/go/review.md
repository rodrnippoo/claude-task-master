# Code Review Assistant

Perform a thorough code review of the current changes or a specified file/PR.

## Usage

```
/go/review [target]
```

- `target` (optional): file path, PR number, or branch name to review. Defaults to staged/unstaged changes.

## What This Does

1. Gathers the diff or file content to review
2. Analyzes code quality, potential bugs, and style issues
3. Checks for consistency with the existing codebase patterns
4. Provides actionable, prioritized feedback

## Review Checklist

When reviewing, check for:

### Correctness
- Logic errors or off-by-one issues
- Unhandled edge cases or error paths
- Race conditions or async/await misuse
- Incorrect variable scoping

### Code Quality
- Functions doing too much (single responsibility)
- Duplicate code that could be extracted
- Magic numbers/strings that should be constants
- Overly complex conditionals that could be simplified

### Security
- Input validation and sanitization
- Exposed secrets or credentials
- SQL injection or XSS vectors (if applicable)
- Insecure dependencies

### Performance
- Unnecessary re-renders or recomputations
- Missing memoization opportunities
- N+1 query patterns
- Large bundle size impacts

### Consistency
- Follows existing naming conventions in this codebase
- Matches error handling patterns used elsewhere
- Uses project utilities/helpers instead of reinventing
- JSDoc/comments match the style in similar files

## Steps

1. If a target is provided, examine that specific file or fetch the PR diff
2. If no target, run `git diff HEAD` and `git diff --cached` to get current changes
3. Read through the changes carefully
4. For each issue found, note:
   - **Severity**: Critical / Major / Minor / Nit
   - **Location**: file:line
   - **Issue**: clear description of the problem
   - **Suggestion**: concrete fix or improvement
5. Summarize overall assessment at the top
6. If changes look good, say so clearly — don't manufacture issues

## Output Format

```
## Review Summary
[1-3 sentence overall assessment]

**Status**: ✅ Approved / ⚠️ Needs Changes / ❌ Blocked

---

### Issues

**[CRITICAL|MAJOR|MINOR|NIT]** `path/to/file.js:42`
> Description of the issue

Suggested fix:
```js
// example fix
```

---

### Positives
[Note things done well — this matters for morale and learning]
```

## Notes

- Be direct but constructive — explain *why* something is an issue
- Prioritize correctness and security over style
- Nits are optional suggestions, not blockers
- If you're unsure about something, flag it as a question rather than an issue
