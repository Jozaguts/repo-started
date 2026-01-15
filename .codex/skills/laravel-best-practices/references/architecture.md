# Architecture and Structure

## Controllers
- Keep controllers as orchestrators: accept a Form Request, call an Action/Service, return a response.
- Avoid query construction and domain rules in controllers.
- Prefer single-action (invokable) controllers for simple endpoints.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Http\Controllers;

use App\Actions\User\RegisterUser;
use App\Data\UserRegistrationData;
use App\Http\Requests\RegisterUserRequest;
use Illuminate\Http\JsonResponse;

final class RegisterUserController
{
    public function __construct(
        private readonly RegisterUser $registerUser,
    ) {}

    public function __invoke(RegisterUserRequest $request): JsonResponse
    {
        $data = UserRegistrationData::fromRequest($request);
        $user = $this->registerUser->handle($data);

        return response()->json(['id' => $user->id], 201);
    }
}
```

## Actions and Services
- Name by intent (verb-noun) and keep a single responsibility.
- Accept DTOs or explicit parameters; return domain objects or value objects.
- Own transactional boundaries when multiple writes are required.

## DTOs
- Use small readonly DTOs for validated request data.
- Prefer explicit construction over passing arrays across boundaries.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Data;

use App\Http\Requests\RegisterUserRequest;

final readonly class UserRegistrationData
{
    public function __construct(
        public string $name,
        public string $email,
        public string $password,
    ) {}

    public static function fromRequest(RegisterUserRequest $request): self
    {
        $data = $request->validated();

        return new self($data['name'], $data['email'], $data['password']);
    }
}
```

## Enums
- Use PHP enums for finite values (status, role, type).
- Cast enums in Eloquent models via `casts`.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Enums;

enum UserStatus: string
{
    case Active = 'active';
    case Suspended = 'suspended';
}
```
