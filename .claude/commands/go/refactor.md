# Refactor Command

Refactor the specified code to improve quality, readability, and maintainability without changing behavior.

## Usage

```
/go/refactor [target] [--scope=<scope>] [--focus=<focus>]
```

## Arguments

- `target` — File, directory, or description of code to refactor (optional, defaults to current context)
- `--scope` — Scope of refactoring: `file`, `module`, `function`, `component` (default: `file`)
- `--focus` — What to focus on: `readability`, `performance`, `duplication`, `types`, `all` (default: `all`)

## What This Does

1. **Analyzes** the target code for issues:
   - Code duplication / repeated patterns
   - Overly complex functions (high cyclomatic complexity)
   - Poor naming conventions
   - Missing or incorrect TypeScript types
   - Inconsistent patterns vs rest of codebase
   - Dead code / unused variables

2. **Plans** the refactoring:
   - Lists specific changes to be made
   - Confirms no behavior changes are intended
   - Identifies any tests that need updating

3. **Executes** the refactoring:
   - Applies changes incrementally
   - Preserves all existing functionality
   - Updates imports/exports as needed

4. **Validates** the result:
   - Runs existing tests to confirm nothing broke
   - Checks TypeScript compilation if applicable
   - Summarizes what changed and why

## Instructions

You are refactoring code in the claude-task-master project. Follow these steps:

### Step 1 — Identify Target

If no target is specified, look at recent changes or ask the user what to refactor. Check:
- The file(s) mentioned in the current task context
- Recently modified files via `git diff --name-only HEAD~1`

### Step 2 — Analyze Code Quality

Read the target file(s) and identify:
- Functions longer than 40 lines that could be extracted
- Repeated code blocks (3+ lines duplicated)
- Variables with unclear names (single letters, abbreviations)
- Missing JSDoc comments on exported functions
- Any `any` types in TypeScript that could be more specific
- Callback hell that could use async/await
- Nested conditionals deeper than 3 levels

### Step 3 — Check Codebase Patterns

Before refactoring, scan 2-3 similar files in the project to understand:
- Naming conventions used (camelCase, snake_case, etc.)
- Error handling patterns
- How modules are structured
- Import ordering conventions

### Step 4 — Apply Refactoring

Make targeted improvements. Prefer:
- Small, focused extractions over large rewrites
- Existing utility functions over writing new ones
- Project conventions over personal preferences

Do NOT:
- Change public API signatures without explicit instruction
- Rename exported symbols without updating all usages
- Remove functionality even if it looks unused
- Add new dependencies

### Step 5 — Run Validation

```bash
# Run tests related to changed files
npm test -- --testPathPattern="<changed-file-basename>"

# Check TypeScript if applicable
npm run type-check 2>/dev/null || npx tsc --noEmit 2>/dev/null
```

### Step 6 — Summarize

Provide a concise summary:
- What files were changed
- What patterns were improved
- Any follow-up refactoring opportunities noted but not acted on
- Whether tests passed

## Examples

```
/go/refactor src/utils/task-utils.js
/go/refactor --focus=duplication
/go/refactor src/commands/ --scope=module --focus=types
```
