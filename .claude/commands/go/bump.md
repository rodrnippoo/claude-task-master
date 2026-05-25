# Bump Version

Bump the version of the project using changesets.

## Usage

```
/go/bump [patch|minor|major]
```

## Arguments

- `$ARGUMENTS` - Optional version bump type (patch, minor, major). If not provided, will use changeset's interactive mode.

## Instructions

You are helping bump the project version using changesets. Follow these steps carefully:

### 1. Check Current State

First, understand where we are:

```bash
# Check current version
cat package.json | grep '"version"'

# Check pending changesets
ls .changeset/*.md 2>/dev/null | grep -v README.md || echo "No pending changesets"

# Show git status
git status --short
```

### 2. Validate Changesets Exist

If no changesets exist and a bump type was specified, create one:

```bash
# If $ARGUMENTS is provided (patch/minor/major), create a changeset automatically
# Otherwise, remind the user to run /go/changelog first
```

If `$ARGUMENTS` is empty and no changesets exist:
- Remind the user to use `/go/changelog` to create a changeset first
- Or use `/go/version` to manually set a specific version
- Stop here and do not proceed

### 3. Run Version Bump

If changesets exist or a bump type was provided:

```bash
# Apply all pending changesets and bump version
npx changeset version
```

This will:
- Consume all `.changeset/*.md` files
- Update `package.json` version
- Update `CHANGELOG.md`

### 4. Review Changes

```bash
# Show what changed
git diff package.json
git diff CHANGELOG.md

# Show new version
cat package.json | grep '"version"'
```

### 5. Commit the Version Bump

```bash
# Stage version bump files
git add package.json CHANGELOG.md package-lock.json 2>/dev/null

# Remove consumed changeset files from staging
git add .changeset/

# Commit
new_version=$(cat package.json | python3 -c "import sys,json; print(json.load(sys.stdin)['version'])" 2>/dev/null || node -e "console.log(require('./package.json').version)")
git commit -m "chore: bump version to v${new_version}"
```

### 6. Create Git Tag

```bash
# Tag the release
new_version=$(node -e "console.log(require('./package.json').version)")
git tag -a "v${new_version}" -m "Release v${new_version}"

echo "Version bumped to v${new_version}"
echo "Tag v${new_version} created"
echo ""
echo "Next steps:"
echo "  - Run /go/release to publish to npm"
echo "  - Or push manually: git push && git push --tags"
```

## Notes

- This command uses [changesets](https://github.com/changesets/changesets) for version management
- Changesets in `.changeset/` directory are consumed and removed during this process
- The `CHANGELOG.md` is automatically updated with the changeset descriptions
- Use `/go/changelog` to create a new changeset before bumping
- Use `/go/release` after bumping to publish to npm
