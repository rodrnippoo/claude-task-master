# Setup Command

Initialize or configure a project environment, dependencies, and tooling.

## Usage

```
/go/setup [target]
```

## What This Does

This command helps you set up a project from scratch or configure an existing one. It will:

1. **Detect the project type** (Node.js, Python, Go, etc.) based on existing files
2. **Install dependencies** if a package manifest exists
3. **Configure environment variables** by checking for `.env.example` and creating `.env` if missing
4. **Set up git hooks** if a `.husky` or similar config is present
5. **Validate the setup** by running any available health checks or smoke tests

## Steps

### 1. Audit Current State
- Check for `package.json`, `requirements.txt`, `go.mod`, etc.
- Identify missing config files
- Note any obvious misconfigurations

### 2. Environment Setup
- Copy `.env.example` → `.env` if `.env` doesn't exist
- Prompt for any required secrets or API keys that are clearly missing
- Warn about any env vars that look like they still have placeholder values

### 3. Install Dependencies
- Run the appropriate install command for the detected package manager
  - npm/yarn/pnpm for Node.js
  - pip/poetry/uv for Python
  - `go mod download` for Go
- Report any install failures clearly

### 4. Build / Compile Check
- Attempt a dry-run build if applicable
- Surface any compile errors or missing type definitions

### 5. Run Smoke Tests
- If a test suite exists, run a minimal subset to confirm the environment works
- Report pass/fail without going deep into failures (that's for `/go/debug`)

## Arguments

- `[target]` — Optional. Specific part of the project to set up (e.g., `frontend`, `backend`, `database`). Defaults to the whole project.

## Example Prompts

- `/go/setup` — Full project setup
- `/go/setup frontend` — Only set up the frontend workspace
- `/go/setup database` — Initialize and migrate the database

## Notes

- This command is **non-destructive** by default. It won't overwrite existing `.env` files or reset databases unless explicitly asked.
- If you're onboarding to an existing project, run this first before anything else.
- For CI/CD setup, see `/go/deploy` instead.
- If setup fails partway through, re-running is safe — steps are idempotent where possible.
