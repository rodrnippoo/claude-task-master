# Scaffold

Generate boilerplate code, files, or project structures based on a description or pattern.

## Usage

```
/go:scaffold <what to scaffold> [--template <template>] [--dry-run]
```

## Examples

- `/go:scaffold new CLI command called "export" with options for format and output path`
- `/go:scaffold React component for a data table with sorting and pagination`
- `/go:scaffold REST API endpoint for user preferences`
- `/go:scaffold test file for src/utils/parser.js`

## Instructions

You are scaffolding new code or files for the claude-task-master project. Follow these steps:

### 1. Understand the Request

Parse the scaffold request to determine:
- **Type**: component, module, command, test, config, endpoint, etc.
- **Name**: what it should be called
- **Location**: where it should live in the project
- **Dependencies**: what it needs to import or integrate with

### 2. Analyze Existing Patterns

Before generating anything, examine the codebase:
- Look at 2-3 similar existing files to understand conventions
- Check naming patterns (camelCase, kebab-case, etc.)
- Identify common imports and boilerplate used
- Note JSDoc/comment style in use
- Check if there are index files that need updating

```
Read relevant existing files to understand patterns before writing new ones.
```

### 3. Plan the Scaffold

List out what will be created or modified:
- New files to create
- Existing files that need updating (index exports, registries, etc.)
- Any config changes needed

Present this plan and confirm with the user if the scope is large (more than 3 files).

### 4. Generate the Code

Write realistic, functional code — not placeholder stubs:
- Include proper imports matching the project's module system
- Add JSDoc comments for exported functions/classes
- Follow the error handling patterns used elsewhere
- Wire up to existing systems (e.g., register a new command in the command registry)
- Include a basic test file if scaffolding a module

### 5. Dry Run Mode

If `--dry-run` is passed, show what would be created/modified without writing files. Display the full content of each file that would be generated.

### 6. Write Files

Create all planned files. After writing:
- Confirm each file was created successfully
- Note any manual steps the user needs to take
- Suggest a follow-up command if appropriate (e.g., `/go:test` to run tests)

## Output Format

After scaffolding, provide a summary:

```
✅ Scaffolded: <name>

Created:
  - path/to/new-file.js
  - path/to/new-file.test.js

Updated:
  - path/to/index.js (added export)

Next steps:
  - Run /go:test to verify the scaffold works
  - Fill in the TODO sections in new-file.js
```

## Notes

- Prefer extending existing patterns over introducing new ones
- If unsure about placement, ask before creating files in ambiguous locations
- Always check if a similar file already exists before scaffolding
