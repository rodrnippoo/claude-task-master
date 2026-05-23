# Plan Command

Analyze the current state of the project and create a detailed implementation plan before starting work.

## Usage

```
/go/plan [goal or feature description]
```

## What This Does

Before jumping into implementation, this command helps you:
1. Understand the current codebase state
2. Break down the work into clear tasks
3. Identify potential blockers or dependencies
4. Create a structured plan you can execute step by step

## Steps

### 1. Understand the Goal

Read the provided goal or feature description carefully. If none is provided, check:
- Recent git commits for context
- Open tasks in `tasks/tasks.json` if it exists
- Any TODO comments in recently modified files

### 2. Audit Current State

Run these checks to understand where we are:

```bash
# Check git status
git status
git log --oneline -10

# Look at existing tasks if task-master is set up
npx task-master list 2>/dev/null || echo "No task-master tasks found"

# Check for any failing tests
npm test --passWithNoTests 2>/dev/null | tail -20
```

### 3. Identify Files Involved

Based on the goal, identify:
- Files that need to be **created**
- Files that need to be **modified**
- Files that need to be **deleted**
- Dependencies that need to be **added or updated**

### 4. Break Down Into Tasks

Create a numbered list of concrete tasks. Each task should be:
- Small enough to complete in one focused session
- Clear about what "done" looks like
- Ordered by dependency (do blockers first)

Example format:
```
1. [ ] Set up base file structure for X
2. [ ] Implement core logic in Y
3. [ ] Add tests for Y
4. [ ] Wire up Z to use Y
5. [ ] Update documentation
```

### 5. Flag Risks and Unknowns

Call out anything that might cause problems:
- Breaking changes to existing APIs
- Missing information or unclear requirements
- Large refactors that could introduce bugs
- External dependencies or services needed

### 6. Propose the Plan

Present the plan clearly and ask for confirmation before proceeding. Format:

---
**Goal:** [restate the goal]

**Approach:** [1-2 sentence summary of the strategy]

**Tasks:**
[numbered list]

**Risks:**
[bullet list of concerns, or "None identified"]

**Estimated complexity:** [Low / Medium / High]

Shall I proceed with this plan?

---

## Notes

- Don't start implementing until the plan is confirmed
- If the goal is vague, ask clarifying questions before planning
- Prefer smaller, incremental plans over big-bang rewrites
- If task-master is available, offer to create the tasks using `npx task-master add-task`
