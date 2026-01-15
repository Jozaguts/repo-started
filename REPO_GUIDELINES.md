# Repository Guidelines

## Purpose
Single source of truth for cross-project conventions.
Stack-specific rules live in AGENTS.md and override this file.

---

## Project Structure
- Prefer small, focused modules
- One responsibility per folder
- Avoid deep nesting

---

## Code Quality
- Linting and formatting are mandatory
- No commented-out code
- Prefer clarity over cleverness

---

## Commits
Conventional Commits required:

<type>[scope]: description

Types:
- feat, fix, refactor, perf
- test, docs, chore

---

## Pull Requests
- Small PRs (<300 lines when possible)
- Clear description of *why*, not only *what*
- Screenshots required for UI changes

---

## Testing
- Critical paths must be covered
- Tests must be deterministic
- No skipping tests in CI

---

## Security
- No secrets in repo
- Env vars via `.env.example`
- Validate all external input

---

## Documentation
- README only if it adds value
- Prefer examples over long explanations
