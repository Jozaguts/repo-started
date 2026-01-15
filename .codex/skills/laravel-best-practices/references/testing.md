# Testing (Pest Preferred)

## Test Strategy
- Use feature tests for HTTP endpoints and integration flows.
- Use unit tests for Actions/Services and pure logic.
- Prioritize critical paths and error handling.

## Database
- Use `RefreshDatabase` for isolation.
- Build data with model factories.

## Pest Setup
```php
<?php

use Illuminate\Foundation\Testing\RefreshDatabase;

uses(RefreshDatabase::class)->in('Feature');
```

## Examples
Feature test:
```php
<?php

declare(strict_types=1);

use App\Models\User;

it('registers a user', function (): void {
    $payload = [
        'name' => 'Jane',
        'email' => 'jane@example.com',
        'password' => 'super-secret-1234',
    ];

    $response = $this->postJson('/api/register', $payload);

    $response->assertCreated();
    $this->assertDatabaseHas('users', ['email' => 'jane@example.com']);
});
```

Unit test:
```php
<?php

declare(strict_types=1);

use App\Actions\User\RegisterUser;
use App\Data\UserRegistrationData;

it('creates a user from a DTO', function (): void {
    $action = app(RegisterUser::class);
    $data = new UserRegistrationData('Jane', 'jane@example.com', 'secret');

    $user = $action->handle($data);

    expect($user->email)->toBe('jane@example.com');
});
```

## Jobs, Events, and Notifications
- Use `Bus::fake()` or `Queue::fake()` to assert dispatching.
- Use `Event::fake()` for events and listeners.

Example:
```php
use Illuminate\Support\Facades\Bus;

Bus::fake();

// trigger code

Bus::assertDispatched(ProcessReport::class);
```
