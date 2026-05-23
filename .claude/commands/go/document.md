# Document

Generate or update documentation for the specified code, module, or entire project.

## Usage

```
/go/document [target]
```

## Arguments

- `target` (optional): Specific file, directory, function, or module to document. Defaults to changed files.

## What This Does

1. **Analyzes** the target code to understand its purpose, inputs, outputs, and behavior
2. **Checks existing docs** to avoid duplication and maintain consistency
3. **Generates documentation** appropriate to the context:
   - JSDoc/TSDoc comments for functions and classes
   - README updates for modules or packages
   - Inline comments for complex logic
   - Type annotations where missing
4. **Validates** that examples in docs actually work with current code
5. **Updates** any stale documentation that no longer matches the implementation

## Documentation Style

Follow these conventions:
- Use JSDoc for all exported functions and classes
- Include `@param`, `@returns`, `@throws`, and `@example` tags where relevant
- Keep descriptions concise but complete — explain the *why*, not just the *what*
- For complex algorithms, add a brief explanation of the approach
- Mark deprecated APIs with `@deprecated` and suggest alternatives

## Examples

```
/go/document
```
Documents all files changed since last commit.

```
/go/document src/utils/taskParser.js
```
Generates JSDoc for all functions in taskParser.js.

```
/go/document src/commands/
```
Documents all files in the commands directory and updates the directory README if present.

```
/go/document --readme
```
Focuses on updating the top-level README.md based on current project state.

## Output

- Modified source files with added/updated JSDoc comments
- Updated README files where applicable
- A summary of what was documented and any gaps found

## Notes

- Does not remove existing documentation unless it's clearly wrong or outdated
- If a function's behavior is ambiguous, will ask for clarification before documenting
- Skips test files unless explicitly targeted
- Respects `.gitignore` and won't document build artifacts
