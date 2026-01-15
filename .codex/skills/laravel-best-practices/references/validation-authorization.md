# Validation and Authorization

## Form Requests
- Use Form Requests for validation and authorization, not inline controller rules.
- Normalize input in `prepareForValidation()` when needed.
- Validate enums with `Rule::enum()`.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Http\Requests;

use App\Enums\UserStatus;
use App\Models\User;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rule;

final class RegisterUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        return $this->user()?->can('create', User::class) ?? false;
    }

    public function rules(): array
    {
        return [
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'email', 'max:255', 'unique:users,email'],
            'password' => ['required', 'string', 'min:12'],
            'status' => ['nullable', Rule::enum(UserStatus::class)],
        ];
    }
}
```

## Policies and Gates
- Use Policies for model-specific authorization.
- Use Gates for cross-cutting checks not tied to a model.
- Enforce authorization explicitly in controllers or actions.

Example:
```php
<?php

declare(strict_types=1);

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\JsonResponse;

final class UserController
{
    public function destroy(User $user): JsonResponse
    {
        $this->authorize('delete', $user);
        $user->delete();

        return response()->json([], 204);
    }
}
```

## Validation Output
- Use `$request->validated()` for trusted input.
- Avoid `$request->all()` or unvalidated mass assignment.
