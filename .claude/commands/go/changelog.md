# Generate Changelog

Generate a structured changelog for the project based on git history, commits, and changesets.

## Instructions

1. **Gather git history**
   - Run `git log --oneline --no-merges` to get recent commits
   - Check for existing changeset files in `.changeset/` directory
   - Review `CHANGELOG.md` if it exists to understand current format

2. **Categorize changes** by type:
   - 🚀 **Features** (`feat:`) — new functionality
   - 🐛 **Bug Fixes** (`fix:`) — bug fixes
   - 🔧 **Improvements** (`chore:`, `refactor:`) — internal improvements
   - 📚 **Documentation** (`docs:`) — docs updates
   - ⚡ **Performance** (`perf:`) — performance improvements
   - 🔒 **Security** (`security:`) — security patches
   - 💥 **Breaking Changes** — any breaking API changes

3. **Determine version bump** based on changesets or commit types:
   - Major: breaking changes present
   - Minor: new features, no breaking changes
   - Patch: only fixes and chores

4. **Format the changelog entry**:
   ```markdown
   ## [version] - YYYY-MM-DD

   ### 🚀 Features
   - Description of feature (#PR or commit)

   ### 🐛 Bug Fixes
   - Description of fix (#PR or commit)

   ### 🔧 Improvements
   - Description of improvement
   ```

5. **Handle edge cases**:
   - If no commits since last tag, note "No changes"
   - If changesets exist, use those descriptions (they're more human-friendly)
   - Deduplicate entries that reference the same change
   - Skip CI/tooling-only commits unless significant

6. **Output options** based on `$ARGUMENTS`:
   - No args: preview changelog entry in terminal
   - `--write`: prepend to `CHANGELOG.md`
   - `--version <x.y.z>`: use specific version instead of auto-detecting
   - `--since <tag>`: generate from a specific git tag
   - `--format json`: output as JSON instead of markdown

## Commands to run

```bash
# Get latest tag
git describe --tags --abbrev=0 2>/dev/null || echo "no-tags"

# Get commits since last tag
git log $(git describe --tags --abbrev=0 2>/dev/null)..HEAD --oneline --no-merges

# List pending changesets
ls .changeset/*.md 2>/dev/null | grep -v README

# Get current package version
node -p "require('./package.json').version"
```

## Notes
- Prefer changeset descriptions over raw commit messages when available
- Keep entries concise — one line per change
- Link to PRs/issues when commit messages reference them
- Group multiple small fixes under a single entry if they're related
