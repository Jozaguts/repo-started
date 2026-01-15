# Security

## Input and Mass Assignment
- Use `$request->validated()` for trusted input.
- Set `$fillable` or `$guarded` on Eloquent models.
- Avoid `$request->all()` or passing raw input to `create()`.

## Authorization
- Enforce policies or gates for protected actions.
- Prefer policy checks inside Form Requests for consistency.

## Rate Limiting
- Use route middleware `throttle` or `RateLimiter::for()` for custom limits.

Example:
```php
use Illuminate\Cache\RateLimiting\Limit;
use Illuminate\Support\Facades\RateLimiter;

RateLimiter::for('login', function ($request) {
    return Limit::perMinute(5)->by($request->ip());
});
```

## Security Headers
- Add basic security headers if missing: `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`.
- Consider a tailored `Content-Security-Policy` for web UIs.
