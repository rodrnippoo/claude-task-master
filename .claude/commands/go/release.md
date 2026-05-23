# Release Command

Automate the release process including version bumping, changelog generation, tagging, and publishing.

## Usage

```
/go/release [type] [options]
```

## Arguments

- `type` - Release type: `patch`, `minor`, `major`, or `prerelease` (default: `patch`)
- `options` - Additional flags like `--dry-run`, `--no-publish`, `--tag <tag>`

## Process

<steps>

### 1. Pre-release Validation

Before starting the release:
- Verify you're on the correct branch (usually `main` or `master`)
- Check for uncommitted changes — warn if any exist
- Ensure all tests pass by running the test suite
- Confirm the working tree is clean
- Validate that the remote is up to date

```bash
git status
git fetch origin
git diff origin/main
npm test
```

### 2. Determine Version

Analyze the release type and current version:
- Read current version from `package.json`
- Calculate next version based on semver rules
- If changesets are present, use `changeset version` to determine bump
- Show the user what the new version will be and ask for confirmation

```bash
cat package.json | grep version
npx changeset status
```

### 3. Update Changelog

Generate or update the changelog:
- Run `/go/changelog` logic to compile changes since last tag
- Update `CHANGELOG.md` with the new version section
- Include commits grouped by type (feat, fix, chore, etc.)
- Reference any linked issues or PRs

```bash
npx changeset version
# or
git log $(git describe --tags --abbrev=0)..HEAD --oneline
```

### 4. Bump Version

Update version numbers across the project:
- Update `package.json` version field
- Update `package-lock.json` if present
- Run any pre-version scripts defined in package.json
- Stage all version-related file changes

```bash
npm version [patch|minor|major] --no-git-tag-version
# or with changesets:
npx changeset version
```

### 5. Commit Release

Create the release commit:
- Stage all modified files (`package.json`, `CHANGELOG.md`, etc.)
- Create a commit with the message format: `chore: release v{version}`
- This commit should only contain version bump and changelog changes

```bash
git add package.json package-lock.json CHANGELOG.md
git commit -m "chore: release v{version}"
```

### 6. Tag the Release

Create a git tag for the release:
- Create an annotated tag: `v{version}`
- Include a brief description in the tag message
- Do not push yet — verify locally first

```bash
git tag -a v{version} -m "Release v{version}"
git show v{version}
```

### 7. Push to Remote

Push the release to the remote repository:
- Push the commit to the current branch
- Push the tag separately
- Verify the push succeeded

```bash
git push origin main
git push origin v{version}
```

### 8. Publish to npm (if applicable)

If this is an npm package:
- Run `npm publish` or `npx changeset publish`
- Use the appropriate dist-tag for prereleases
- Verify the package appears on npm registry

```bash
npx changeset publish
# or
npm publish --access public
```

### 9. Create GitHub Release

Create a GitHub release entry:
- Use the tag created in step 6
- Populate the release notes from the changelog
- Mark as pre-release if applicable
- Attach any build artifacts if needed

```bash
gh release create v{version} --title "v{version}" --notes-file RELEASE_NOTES.md
```

### 10. Post-release Summary

Report what was done:
- Show the new version number
- Link to the GitHub release
- Link to the npm package page if published
- List any follow-up tasks (e.g., update docs site, notify team)

</steps>

## Dry Run Mode

When `--dry-run` is passed, simulate all steps without making changes:
- Show what version would be bumped to
- Preview changelog entries
- Print git commands without executing them
- Skip npm publish entirely

## Notes

- Always confirm with the user before pushing tags or publishing
- If using changesets, defer to `changeset version` and `changeset publish`
- Prereleases should use tags like `alpha`, `beta`, or `rc`
- Never force-push after tagging a release
