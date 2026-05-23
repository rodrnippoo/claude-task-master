# Commit Changes

Create a well-structured git commit for the current staged or unstaged changes.

## Instructions

1. **Check current state**
   - Run `git status` to see what files have changed
   - Run `git diff` to understand what actually changed
   - Run `git diff --staged` if there are already staged changes

2. **Stage changes if needed**
   - If nothing is staged, review the unstaged changes and stage appropriate files
   - Group related changes together — don't mix unrelated fixes in one commit
   - Use `git add <file>` for specific files or `git add -p` for partial staging

3. **Analyze the changes**
   - Identify the primary purpose of the changes (feat, fix, refactor, docs, chore, test)
   - Note which modules or areas are affected
   - Check if there are breaking changes

4. **Craft the commit message**
   - Use conventional commit format: `type(scope): short description`
   - Keep the subject line under 72 characters
   - Add a body if the change needs more explanation (what and why, not how)
   - Reference any relevant issue numbers

5. **Create the commit**
   - Run `git commit -m "<message>"` or open editor for multi-line messages
   - Verify the commit was created with `git log --oneline -1`

## Commit Types

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code restructure without behavior change |
| `docs` | Documentation only changes |
| `test` | Adding or updating tests |
| `chore` | Build process, deps, tooling |
| `perf` | Performance improvement |
| `ci` | CI/CD configuration changes |

## Examples

```
feat(tasks): add support for subtask dependencies
fix(sync): handle missing remote branch gracefully
docs(readme): update installation instructions for v2
refactor(cli): extract command parsing into separate module
```

## Notes

- If changes span multiple concerns, suggest splitting into multiple commits
- Never commit secrets, credentials, or `.env` files
- Check `.gitignore` if you see files that shouldn't be tracked
- After committing, confirm whether to push or leave for the user to push
