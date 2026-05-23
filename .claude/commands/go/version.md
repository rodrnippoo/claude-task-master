# Version Bump Command

Bump the project version, update changelogs, and prepare a release.

## Usage

```
/go/version [patch|minor|major|<version>]
```

## Arguments

- `$ARGUMENTS` - Version bump type or explicit version (default: patch)

## Steps

1. **Determine bump type** from `$ARGUMENTS` (patch, minor, major, or explicit semver)
2. **Check working tree** is clean before proceeding
3. **Run tests** to ensure nothing is broken
4. **Bump version** using appropriate tooling
5. **Update changelog** from changesets or commit history
6. **Commit and tag** the release

## Process

```bash
# Check for uncommitted changes
git status --porcelain

# Run tests first
npm test

# Determine bump type
BUMP_TYPE=${ARGUMENTS:-patch}

# If using changesets
if [ -d ".changeset" ]; then
  npx changeset version
  npx changeset tag
else
  # Standard npm version bump
  npm version $BUMP_TYPE --no-git-tag-version
fi

# Show what changed
git diff package.json
```

## What to Check

- [ ] Working tree is clean (no uncommitted changes)
- [ ] All tests pass before bumping
- [ ] CHANGELOG.md is updated
- [ ] package.json version is correct
- [ ] Git tag matches the new version
- [ ] Any peer dependency version constraints are still valid

## Changeset Workflow

This project uses [changesets](https://github.com/changesets/changesets) for version management.

- Individual changes are tracked in `.changeset/*.md` files
- Run `npx changeset` to create a new changeset for your PR
- Run `npx changeset version` to consume changesets and bump versions
- Run `npx changeset publish` to publish to npm

## After Bumping

1. Review the generated changelog entry
2. Commit with message: `chore: release v<new-version>`
3. Push the commit and tag: `git push && git push --tags`
4. Create a GitHub release if appropriate

## Notes

- Never bump version on a dirty working tree
- Always run tests before releasing
- For pre-release versions use: `npm version prerelease --preid=beta`
- Check that `.changeset` directory has been cleared after consuming changesets
