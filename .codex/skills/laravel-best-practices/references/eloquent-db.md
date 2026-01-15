# Eloquent and Database

## Scopes
- Prefer model scopes to avoid duplicated query logic.
- Keep scope names descriptive and composable.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Models;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;

final class Order extends Model
{
    public function scopePaid(Builder $query): Builder
    {
        return $query->whereNotNull('paid_at');
    }
}
```

## Eager Loading and N+1
- Use `with()` and `loadMissing()` to avoid N+1 queries.
- Prefer `withCount()` or `withSum()` for aggregates.
- Limit selected columns with `select()` where appropriate.

## Transactions
- Wrap multi-write operations with `DB::transaction()`.

Example:
```php
use Illuminate\Support\Facades\DB;

DB::transaction(function () use ($data): void {
    // multiple writes
});
```

## Large Datasets
- Use `chunkById()` or `lazyById()` for large tables.
- Use `cursor()` for streaming when memory is tight.

## Batch Writes
- Prefer `upsert()` or bulk `insert()` when writing many rows.
