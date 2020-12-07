## **Laravel Sanctum Implementation Demo project**
#### RequirementRequirement
Laravel framework
Please follow the link for Laravel installation https://laravel.com/docs/8.x/installation
#### Steps for Sanctum

- Install Sanctum via composer
> composer require laravel/sanctum

- Run migrations do that token tables by Sanctum can be added to database
>php artisan migrate

- Add Sanctum provider to Laravel
> php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"

- Add Sanctum middleware to Laravel's kernel
>app/Http/Kernel.php update the $middlewareGroups['api'] (Already there) just add the following class

	>     \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class

	So the portion should look like this
	>         'api' => [
				\Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
				'throttle:api',
				\Illuminate\Routing\Middleware\SubstituteBindings::class,
			]
		
- Additions to Model (We have to import)
     Class:
        use Laravel\Sanctum\HasApiTokens;
				
     Trait:
        use HasApiTokens
				
 to our Model class
        
	Example:
			    use Laravel\Sanctum\HasApiTokens;
				use Illuminate\Notifications\Notifiable;
				use Illuminate\Database\Eloquent\Factories\HasFactory;
				use Illuminate\Foundation\Auth\User as Authenticatable;

				class User extends Authenticatable
				{
					use HasApiTokens, HasFactory, Notifiable;
- Example to generate a token and return via json

		$token = $user->createToken('x-api-token')->plainTextToken;

        $response = [
            'token' => $token
        ];

        return response($response, 200);
		
- Route for the action require authentication via sectum.

		Route::group(['middleware' => 'auth:sanctum'], function () {
			Route::get('/getAll', [UserController::class, 'list']);
		});
