# Performance

## Query Efficiency
- Eager load relationships to avoid N+1.
- Use `select()` to limit columns.
- Favor `withCount()` or `withSum()` for aggregates.

## Pagination
- Use `paginate()` or `simplePaginate()` for collections returned to clients.

## Large Data Processing
- Use `chunkById()` or `lazyById()` for long-running data work.
- Avoid loops that run queries per item; batch and eager load.

## Caching
- Use `Cache::remember()` for read-heavy data.
- Prefer `Cache::tags()` when invalidation needs to be scoped.

## Queues for Heavy Tasks
- Offload slow work to queues; see `references/queues-events.md` for details.
