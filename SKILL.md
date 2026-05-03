---
name: clean-code
description: Use for every coding task to control flow complexity. Enforces a complexity gate that treats each branch as an added logical path requiring explicit business justification, especially during implementation and code review.
---

# clean-code

Keep code correct by controlling flow complexity.

## When to use

Use this skill for every coding task, especially implementation, refactoring, debugging, and code review.

## Principles

1. **Complexity is the number of paths**: Every branch multiplies the paths that must be understood, tested, and proven to match the intended business semantics. Do not add a branch unless its necessity is explicit.
2. **Compatibility is debt**: Avoid adding code that is only compatible with existing systems or conventions without a clear business reason, as it increases complexity without providing value. Don't compatible-ize unless necessary for a specific business need.
3. **Minimal Error Handling**: Catch only errors the code knows how to handle, and keep error recovery separate from ordinary business flow. Avoid adding error handling that hides unknown states or swallows errors without a clear recovery strategy. Don't log or catch errors everywhere.
4. **Divide and Conquer**: When you need to solve a problem with multiple items, consider whether to solve it with a single path that iterates over the items, or with multiple paths that each handle one item. Don't add paths for each item unless there is a clear business reason to treat them separately.

## Instructions

1. Treat complexity as the number of possible logical paths through a program.

   Every branch multiplies the paths that must be understood, tested, and proven to match the intended business semantics. Humans and AI agents both fail when there are too many paths to hold in attention. Do not add a branch unless its necessity is explicit.

2. Treat every logical divergence as a branch.

   Branches include `if`, `else`, `switch`, ternaries, `try`, `catch`, `throw`, loops, `break`, `continue`, `return`, `await`, callbacks, forks, spawned work, feature flags, boolean parameters, fallback paths, and any other point where execution can diverge.

3. Apply the complexity gate before changing code.

   For every new or modified branch, require a sufficient reason:
   - What business rule, input class, or failure mode does this path represent?
   - Why must it be a separate path instead of part of the main path?
   - Can this path be deleted, merged, shortened, or moved behind a clearer boundary?
   - How will this path be verified by tests, existing behavior, or explicit reasoning?

   If a branch lacks a sufficient reason, treat it as a defect. Do not normalize it as style, preference, or harmless flexibility.

4. Implement with the fewest necessary paths.
   - Prefer the smallest correct change.
   - Keep the main path straight and visible.
   - Make necessary branches short, local, named when useful, and easy to test.
   - Avoid nested branches unless each nested decision has a clear independent semantic reason.
   - Do not add boolean or mode parameters unless each mode is a real business concept.
   - Do not add fallback behavior that hides unknown states or swallows errors.
   - Catch only errors the code knows how to handle, and keep error recovery separate from ordinary business flow.
   - Split code when one function is forced to manage unrelated paths.
   - Use data structure, table-driven dispatch, or polymorphism only when it reduces path reasoning.

5. Keep errors visible, contextual, and simple.

   Prefer ordinary exceptions over error-code return values such as `-1`, `null`, or `undefined` when representing failure. Throw `Error` instances with enough string information to debug the failure:
   - Include an error category, such as `ValidationError`, `NetworkError`, or `ConfigError`.
   - Include the relevant context values, such as IDs, parameters, status codes, or state names.
   - Avoid throwing strings or constructing complex error objects with protocols such as `error.code`, `error.type`, or custom metadata unless the project already has a concrete, enforced contract for them.
   - Avoid custom error classes unless they are required by an external API, framework integration, or existing project convention.
   - Treat caught errors as `unknown`; do not assume they follow a local metadata shape.

6. Catch errors only for one of four explicit responsibilities.

   A `catch` is a branch and must have one of these meanings:
   - Recover with a real fallback, such as cached data or a looser parser.
   - Retry an operation that is expected to fail transiently.
   - Add missing context and rethrow while preserving the original error with `cause` or an existing project helper.
   - Present, report, or contain the error at a boundary such as API, GUI, CLI, worker, delegate, or error boundary code.

   Do not catch only to log, ignore, translate without preserving the original error, or make the code look defensive. Log where the error is presented or reported, not at every layer. If wrapping is useful, prefer an existing helper such as `newError` or `scopeError`; otherwise use `new Error(message, { cause: error })` where supported.

7. Review flow complexity before style.

   List findings by severity:
   - `blocker`: a new or existing path lacks sufficient business meaning, hides failure, or makes correctness hard to prove.
   - `non-blocker`: a path is necessary but could be shorter, clearer, more local, or better tested.

   For each finding, name the branch and explain:
   - The paths it creates.
   - Why those paths are insufficiently justified or hard to reason about.
   - The concrete simplification: delete, merge, shorten, split, move, test, or clarify the branch.

8. Report the complexity result for non-trivial changes.

   Include:
   - New paths added.
   - Why each path is necessary.
   - How the paths were kept local and verified.

   If no new paths were added, say so.
