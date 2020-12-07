## **Laravel Sanctum Implementation Demo project**
#### RequirementRequirement
Laravel framework
Please follow the link for Laravel installation https://laravel.com/docs/8.x/installation
#### Steps for Sanctum

- Install Sanctum via composer
```bash
composer require laravel/sanctum
```
- Run migrations do that token tables by Sanctum can be added to database
```bash
php artisan migrate
```
- Add Sanctum provider to Laravel
```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```
- Add Sanctum middleware to Laravel's kernel
```php
app/Http/Kernel.php update the $middlewareGroups['api'] (Already there) just add the following class
\Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class
```
So the portion should look like this
```php
'api' => [
    Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
    'throttle:api',
    Illuminate\Routing\Middleware\SubstituteBindings::class,
]
```
- Additions to Model (We have to import)
    Class:
    ```php
        use Laravel\Sanctum\HasApiTokens;
	```
    Trait:
    ```php
        use HasApiTokens
    ```
    Example:
    ```php
	use Laravel\Sanctum\HasApiTokens;
	use Illuminate\Notifications\Notifiable;
	use Illuminate\Database\Eloquent\Factories\HasFactory;
    use Illuminate\Foundation\Auth\User as Authenticatable;

	class User extends Authenticatable
	{
        use HasApiTokens, HasFactory, Notifiable;
    ```

- Example to generate a token and return via json
```php
$token = $user->createToken('x-api-token')->plainTextToken;
$response = [
    'token' => $token
];
return response($response, 200);
```

- Route for the action require authentication via sectum.
```php
Route::group(['middleware' => 'auth:sanctum'], function () {
	Route::get('/getAll', [UserController::class, 'list']);
});
```

Add parameter to http header while requesting
```http
Authorization:Bearer <token>
Example
```http
Authorization:Bearer jABH69Z2wMcDBh4iRW8ySaLvwHZPJEyuPX9gSXbQ