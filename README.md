<h3>Laravel Sanctum Implementation Demo project</h3>

<ul>
    <li>
        <strong>Requirement</strong>
        <pre>Laravel framework</pre>
        Please follow the link for Laravel installation
        https://laravel.com/docs/8.x/installation
     </li>
</ul>

<strong>Steps for Sanctum</strong>
<ul>
    <li>
        Install Sanctum via composer
        <pre>composer require laravel/sanctum</pre>
     </li>
    <li>
        Run migrations do that token tables by Sanctum can be added to database
        <pre>php artisan migrate</pre>
     </li>
    <li>
        Add Sanctum provider to Laravel
        <pre>php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"</pre>
     </li>
    <li>
        Add Sanctum middleware to Laravel's kernel<br/>
        app/Http/Kernel.php inside the <pre>$middlewareGroups[]</pre> (Already there) just add the following class
        <pre>\Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,</pre>
     </li>
</ul>