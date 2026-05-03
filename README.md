# clean-code

Clean Code skills for agent-assisted programming.

This repository provides a lightweight skill package that helps coding agents keep changes focused, readable, and maintainable. It is intended for day-to-day software work where the agent should favor small correct edits, clear reasoning, and practical engineering tradeoffs.

## What It Helps With

- Writing code that is easier to read, review, and change.
- Keeping implementations small instead of adding unnecessary abstraction.
- Improving naming, structure, and error handling.
- Reviewing code with attention to bugs, regressions, and missing tests.
- Communicating changes clearly while collaborating in an existing codebase.

## Install

```sh
npx skills add zccz14/clean-code
```

## Guiding Principles

- Prefer the smallest correct change.
- Optimize for clarity before cleverness.
- Avoid speculative abstractions and unused flexibility.
- Preserve existing project conventions unless there is a clear reason to change them.
- Verify behavior with relevant tests or checks whenever possible.

## License

MIT
