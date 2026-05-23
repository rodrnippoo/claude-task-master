# Optimize

Analyze and optimize code for performance, memory usage, or bundle size.

## Usage

```
/go/optimize [target] [--type=performance|memory|bundle|all]
```

## Arguments

- `target` - File, directory, or module to optimize (defaults to current context)
- `--type` - Type of optimization to focus on (defaults to `all`)

## What This Does

1. **Profile the target** - Identify bottlenecks and inefficiencies
2. **Analyze patterns** - Look for common anti-patterns (unnecessary re-renders, memory leaks, large imports)
3. **Suggest & apply fixes** - Make targeted improvements with explanations
4. **Verify improvements** - Confirm optimizations don't break existing behavior

## Optimization Types

### Performance
- Reduce algorithmic complexity (O(n²) → O(n log n) etc.)
- Eliminate redundant computations and loops
- Add memoization where appropriate
- Optimize database queries and API calls
- Lazy load heavy dependencies

### Memory
- Fix memory leaks (event listeners, subscriptions, closures)
- Reduce object allocations in hot paths
- Use streaming instead of buffering large datasets
- Clean up timers and intervals

### Bundle
- Replace heavy libraries with lighter alternatives
- Convert to named imports for better tree-shaking
- Identify and remove dead code
- Split large modules into smaller chunks

## Process

```
Think through:
1. What is the current performance characteristic?
2. Where are the bottlenecks?
3. What's the simplest fix with the most impact?
4. Will this change break any existing tests or behavior?
5. How do we measure the improvement?
```

## Examples

```
/go/optimize src/utils/parser.js --type=performance
/go/optimize src/components/ --type=memory  
/go/optimize --type=bundle
```

## Notes

- Always run tests after optimization to catch regressions
- Document non-obvious optimizations with comments explaining *why*
- Prefer readability over micro-optimizations unless profiling shows it matters
- For bundle optimizations, check bundle analyzer output before and after
