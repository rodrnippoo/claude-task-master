# Analyze

Perform a deep analysis of the codebase, a specific file, or a particular aspect of the project.

## Usage

```
/go/analyze [target] [--focus=<area>]
```

## Arguments

- `target` (optional): File path, directory, or feature area to analyze. Defaults to the current task context.
- `--focus` (optional): Specific aspect to focus on. Options: `performance`, `security`, `complexity`, `dependencies`, `patterns`, `coverage`

## What This Does

1. **Identify the analysis target** from arguments or current task context
2. **Read relevant files** and gather context about the area being analyzed
3. **Check task context** via `task-master show` to understand current work
4. **Perform structured analysis** based on the focus area or a general audit
5. **Report findings** with actionable recommendations
6. **Optionally create subtasks** for any issues found that need addressing

## Analysis Areas

### General (default)
- Code structure and organization
- Naming conventions and consistency
- Error handling coverage
- Code duplication
- Module coupling and cohesion

### Performance (`--focus=performance`)
- Inefficient loops or algorithms
- Unnecessary re-renders or recomputations
- Memory leak risks
- Bundle size concerns
- Async/await usage patterns

### Security (`--focus=security`)
- Input validation gaps
- Exposed secrets or credentials
- Dependency vulnerabilities
- Authentication/authorization issues
- SQL injection or XSS risks

### Complexity (`--focus=complexity`)
- Functions exceeding reasonable length
- Deep nesting levels
- Cyclomatic complexity hotspots
- Hard-to-test code paths

### Dependencies (`--focus=dependencies`)
- Unused imports or packages
- Circular dependencies
- Outdated packages with known issues
- Over-reliance on heavy libraries

### Patterns (`--focus=patterns`)
- Inconsistent design patterns
- Anti-patterns in use
- Opportunities to apply better abstractions

### Coverage (`--focus=coverage`)
- Untested code paths
- Missing edge case tests
- Integration test gaps

## Output Format

The analysis will produce:
- **Summary**: High-level overview of findings
- **Issues**: Categorized list with severity (critical/high/medium/low)
- **Recommendations**: Specific, actionable steps to address findings
- **Quick wins**: Low-effort, high-impact improvements

## Example

```
/go/analyze src/core/task-manager.js --focus=complexity
```

This will analyze the task manager module specifically for complexity issues and suggest refactoring opportunities.
