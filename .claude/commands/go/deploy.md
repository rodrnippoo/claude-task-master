# Deploy Command

Deploy the current changes to the target environment.

## Usage

```
/go/deploy [environment]
```

## Arguments

- `environment` (optional): Target environment (`staging`, `production`). Defaults to `staging`.

## What This Does

1. **Pre-flight checks** — Verifies the working tree is clean and all tests pass
2. **Build verification** — Ensures the project builds successfully
3. **Environment validation** — Confirms target environment config exists
4. **Deploy** — Runs the appropriate deploy command for the environment
5. **Post-deploy verification** — Checks health endpoints or smoke tests

## Steps

### 1. Pre-flight

```bash
# Check for uncommitted changes
git status --porcelain

# Run tests
npm test

# Build the project
npm run build
```

If any of these fail, **stop and report the issue**. Do not proceed with deployment.

### 2. Determine Environment

- If `$ARGUMENTS` is `production`, deploy to production
- Otherwise, default to `staging`
- Warn the user before deploying to production and ask for confirmation

### 3. Deploy

```bash
# For staging
npm run deploy:staging

# For production (only after explicit confirmation)
npm run deploy:production
```

If no deploy scripts exist in `package.json`, check for:
- A `Makefile` with `deploy` targets
- A `deploy.sh` script in the root
- CI/CD config files (`.github/workflows/deploy.yml`, etc.)

Report what you find and suggest the appropriate command.

### 4. Post-Deploy

After a successful deploy:
- Report the deployed version (from `package.json`)
- Note the git commit SHA that was deployed
- List any environment-specific notes

## Error Handling

- If tests fail: Show failing test output and stop
- If build fails: Show build errors and stop
- If deploy fails: Show error output and suggest rollback steps
- Never force-push or skip checks to unblock a deploy

## Notes

- Always prefer deploying from a clean git state
- Tag the release commit if deploying to production: `git tag v$(node -p "require('./package.json').version")`
- Check `CHANGELOG.md` or recent commits to summarize what's being deployed
