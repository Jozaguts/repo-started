---
name: laravel-best-practices
description: Laravel 12 + PHP 8.3 best-practices playbook for architecture, validation/authorization, Eloquent/DB access, testing, performance, security, and jobs/queues/events. Use when asked to design, implement, refactor, or review Laravel endpoints, services/actions, policies, Form Requests, Eloquent queries, tests (Pest), background jobs, or performance/security improvements.
---

# Laravel Best Practices

## Overview
You MUST provide concise, production-ready guidance. Prefer minimal, maintainable solutions over clever abstractions. for Laravel 12 projects with strict types, thin controllers, and a domain-first approach. Default to English in outputs unless the user requests Spanish.

This skill MUST be used together with `repo-guidelines`.
If any conflict exists, this skill overrides general repository rules.


## Operating Principles
- Use `declare(strict_types=1);` in PHP files.
- Keep controllers thin; move business logic to Actions/Services/Domain classes.
- Validate with Form Requests; authorize with Policies/Gates.
- Prefer Eloquent scopes over duplicated query logic.
- Optimize DB access (eager load, avoid N+1, batch operations).
- Cover critical paths with deterministic tests (Pest preferred).
- Prefer testing behavior over implementation details.
- Avoid mocking Eloquent unless strictly necessary.

## Hard Rules (Non-Negotiable)

- No business logic in Controllers or Blade views.
- No accessing request data outside Form Requests.
- No string-based states: use PHP Enums.
- No mass assignment without `$fillable` or guarded review.
- No lazy loading in production code paths.
- No database queries inside loops.
- No side effects in synchronous HTTP requests when a queue is appropriate.

## Workflow Decision Tree
- New endpoint or feature: use "Endpoint Implementation" and `references/validation-authorization.md`.
- Refactor controller or query: use "Controller Refactor" and `references/architecture.md`, `references/eloquent-db.md`.
- Background processing: use "Background Job/Queue" and `references/queues-events.md`.
- Performance or reliability concerns: use "Performance Fix" and `references/performance.md`.
- Security concerns: use "Security Pass" and `references/security.md`.
- Testing request: use `references/testing.md` and align unit vs feature.

## Core Playbooks

### Endpoint Implementation
1. Identify request/response contract and domain action.
2. Create Form Request with rules and `authorize()`.
3. Implement Action/Service using DTOs/enums as needed.
4. Use model policies (`$this->authorize`) or gates for access.
5. Keep controller to orchestration and response.
6. Add feature test(s) and factory data.

### Controller Refactor
1. Extract business logic into Action/Service.
2. Replace inline validation with Form Request.
3. Move repeated queries to model scopes.
4. Add eager loading and remove N+1.
5. Wrap multi-write operations in transactions.
6. Add or adjust tests.

### Background Job/Queue
1. Define job implementing `ShouldQueue`.
2. Ensure idempotency and safe retries/backoff.
3. Add structured logging and failure handling.
4. Write tests using `Bus::fake()` or `Queue::fake()`.
5. Prefer events for decoupled side effects.

### Performance Fix
1. Inspect queries and relationships (N+1, overfetching).
2. Add eager loading, `select()` columns, and scopes.
3. Use pagination and chunking for large datasets.
4. Add caching where read-heavy and stable.
5. Add tests for critical behavior.

### Security Pass
1. Use `$request->validated()` and mass assignment protection.
2. Enforce policies/gates and rate limits.
3. Sanitize and validate input boundaries and enums.
4. Add basic security headers if missing.
5. Update tests for permissions and limits.

## References
Load the following references based on the task:
- `references/architecture.md` for controller/service/DTO/enum patterns.
- `references/validation-authorization.md` for Form Requests and policies/gates.
- `references/eloquent-db.md` for scopes, eager loading, and transactions.
- `references/testing.md` for Pest patterns, factories, and database resets.
- `references/performance.md` for caching, pagination, and batching.
- `references/security.md` for mass assignment, rate limiting, and headers.
- `references/queues-events.md` for jobs, retries/backoff, idempotency.

## Output Expectations
- Propose the minimal, maintainable change set.
- Mention relevant tests to add or update.
- Call out any assumptions or missing context.
