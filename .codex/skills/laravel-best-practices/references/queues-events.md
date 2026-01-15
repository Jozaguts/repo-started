# Jobs, Queues, and Events

## Jobs
- Implement `ShouldQueue` for background work.
- Set retry behavior with `tries`, `backoff`, `retryUntil`, and `timeout`.
- Make jobs idempotent; avoid duplicate side effects.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Jobs;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

final class ProcessReport implements ShouldQueue
{
    use Dispatchable;
    use InteractsWithQueue;
    use Queueable;
    use SerializesModels;

    public int $tries = 3;
    public int $timeout = 120;

    public function backoff(): array
    {
        return [30, 120, 300];
    }

    public function handle(): void
    {
        // process report
    }
}
```

## Idempotency
- Use `ShouldBeUnique` or an application-level lock to prevent duplicates.
- Check existing state before executing side effects.

## Logging and Failures
- Log key state transitions with context.
- Implement `failed()` for cleanup or alerts.

## Events and Listeners
- Prefer events for decoupled side effects.
- Queue listeners that perform heavy work.
