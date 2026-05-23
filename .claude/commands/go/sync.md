# Sync Task Master State

Synchronize the local task state with the latest changes from the repository and ensure everything is up to date.

## What this does

1. Pulls latest changes from the remote repository
2. Re-reads and validates the tasks.json file
3. Checks for any drift between task statuses and actual code state
4. Updates task dependencies if needed
5. Reports any inconsistencies found

## Steps

1. **Check git status** — make sure working tree is clean or stash changes
   ```bash
   git status
   git stash (if needed)
   ```

2. **Pull latest** — fetch and merge remote changes
   ```bash
   git pull origin main
   ```

3. **Validate tasks** — run task-master validation
   ```bash
   task-master validate-dependencies
   task-master list --with-subtasks
   ```

4. **Check for completed work** — scan recent commits and compare against task statuses
   - Look at `git log --oneline -20` for clues about what got done
   - Cross-reference with tasks marked `in-progress` or `pending`
   - If a task looks done based on commits, flag it for review

5. **Report drift** — summarize any mismatches found between:
   - Tasks marked `done` but code not merged
   - Tasks marked `pending` but implementation exists
   - Broken dependencies (task depends on something that was removed)

6. **Pop stash if needed**
   ```bash
   git stash pop
   ```

## Output

After running, provide a summary like:

```
✅ Synced with remote (X commits pulled)
📋 Tasks reviewed: N
⚠️  Potential drift detected:
  - Task #12 marked in-progress but no recent commits reference it
  - Task #7 dependency on Task #3 may be stale
✅ No broken dependencies found
```

## Notes

- This command is read-only by default — it won't automatically change task statuses
- If you want to auto-update statuses, confirm with the user first
- Always prefer explicit over implicit when updating task state
