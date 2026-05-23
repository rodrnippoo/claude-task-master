# Migrate

Run database migrations or data transformation tasks for the project.

## Usage

```
/go/migrate [options]
```

## Arguments

- `$ARGUMENTS` - Optional migration target, direction (up/down), or specific migration name

## Instructions

You are helping run migrations for the claude-task-master project. Analyze the current state and execute the appropriate migration steps.

### Step 1: Understand the Migration Context

First, identify what needs to be migrated:
- Check `$ARGUMENTS` for a specific migration target or direction
- If no arguments provided, look for pending migrations
- Inspect the project structure for migration files or schema changes

```bash
# Check for migration-related files
find . -name '*.migration.*' -o -name 'migrations/' -o -name 'migrate.js' 2>/dev/null | head -20

# Check package.json for migration scripts
cat package.json | grep -A5 '"scripts"'
```

### Step 2: Review Current State

Before running any migration:
1. Check git status to understand what has changed
2. Look for schema files, config files, or data files that need updating
3. Identify if this is a tasks.json format migration, config schema update, or dependency migration

```bash
git status
git log --oneline -10
```

### Step 3: Identify Migration Type

For claude-task-master, migrations may include:

**Tasks JSON Schema Migration**
- Check if `tasks/tasks.json` format needs updating
- Look for version fields or structural changes in task objects
- Compare against current schema expectations

**Config Migration**
- Check `.taskmasterconfig` or similar config files
- Identify deprecated fields that need renaming/removing
- Add new required fields with defaults

**Dependency Migration**
- Review `package.json` for outdated dependencies
- Check for breaking changes in major version bumps

### Step 4: Create Migration Plan

Before executing, outline:
1. What will change
2. What could break
3. How to rollback if needed
4. Whether a backup is needed

```bash
# Backup tasks if migrating task data
if [ -f tasks/tasks.json ]; then
  cp tasks/tasks.json tasks/tasks.json.backup.$(date +%Y%m%d_%H%M%S)
  echo "Backup created"
fi
```

### Step 5: Execute Migration

Run the migration based on what was identified:

```bash
# If npm migration script exists
npm run migrate 2>/dev/null || echo "No npm migrate script found"

# If using task-master CLI
npx task-master migrate 2>/dev/null || true
```

For manual migrations, make the changes programmatically and verify each step.

### Step 6: Validate Results

After migration completes:
1. Verify the migrated data/config is valid
2. Run a quick sanity check on task listing if tasks were migrated
3. Check for any error logs

```bash
# Validate tasks still load correctly
npx task-master list 2>/dev/null | head -20 || echo "Could not validate via CLI"

# Check for obvious JSON errors if tasks.json was modified
if [ -f tasks/tasks.json ]; then
  node -e "JSON.parse(require('fs').readFileSync('tasks/tasks.json', 'utf8')); console.log('tasks.json is valid JSON')" 2>&1
fi
```

### Step 7: Report

Summarize what was done:
- What migration was run
- Files that were changed
- Any warnings or issues encountered
- Next steps if manual follow-up is needed
- How to rollback if something went wrong

## Notes

- Always create backups before migrating task data
- Test migrations in a branch before applying to main
- Document any schema changes in the changeset
