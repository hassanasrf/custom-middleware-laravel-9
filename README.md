# Create Custom Middleware In Laravel 9

Step 1 : Create Middleware
In this step, we will create custom middleware. So, run the below command in your terminal. 

```php
php artisan make:middleware RoleType
```
After running the above command, you will find one file on the app/Http/Middleware/RoleType.php location and you have to add below code.
```php
<?php

namespace App\Http\Middleware;

use Closure;

class RoleType
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->type != 'admin') {
            return response()->json('You do not have access!!');
        }

        return $next($request);
    }
}
```

Step 2 : Register This Middleware on Kernel File
Now, we have to register this middleware on the kernel file.
```php
app/Http/Kernel.php

 protected $routeMiddleware = [
        ...
        'roleType' => \App\Http\Middleware\RoleType::class,
    ];
```
