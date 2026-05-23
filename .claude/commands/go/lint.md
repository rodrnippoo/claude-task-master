# Lint & Format

Run linting and formatting checks on the codebase, auto-fix what's safe to fix, and report issues that need manual attention.

## Steps

1. **Detect project setup**
   - Check for ESLint config (`.eslintrc*`, `eslint.config.*`)
   - Check for Prettier config (`.prettierrc*`, `prettier.config.*`)
   - Check for Biome, oxlint, or other linters in `package.json`
   - Look at `scripts` in `package.json` for existing lint/format commands

2. **Run formatter first**
   - If Prettier is configured: `npx prettier --write .` (or use the project's script)
   - If Biome is configured: `npx biome format --write .`
   - Stage formatted changes separately if in a git repo

3. **Run linter with auto-fix**
   - If ESLint is configured: `npx eslint . --fix`
   - If Biome is configured: `npx biome lint --write .`
   - Capture output for review

4. **Report remaining issues**
   - Run linter without `--fix` to get final error/warning count
   - Group issues by file and rule
   - Distinguish errors (blocking) from warnings (non-blocking)

5. **Summarize results**
   - Files modified by formatter
   - Files modified by linter auto-fix
   - Remaining errors that need manual fixes
   - Remaining warnings to consider

## Arguments

- `$ARGUMENTS` — optional path or glob to lint specific files/directories (e.g. `src/` or `src/**/*.ts`)

## Notes

- If `$ARGUMENTS` is provided, scope all lint/format commands to that path
- Do not auto-fix rules flagged as potentially unsafe
- If there are staged git changes, warn before modifying files
- Prefer project scripts (`npm run lint`, `npm run format`) over direct binary calls when available
- For TypeScript projects, also check for `tsc --noEmit` type errors and include in summary
