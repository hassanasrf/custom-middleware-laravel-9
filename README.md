# Create Custom Middleware In Laravel 9

Step 1 : Create Middleware <br>
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

Step 2 : Register This Middleware on Kernel File <br>
Now, we have to register this middleware on the kernel file.
```php
app/Http/Kernel.php

 protected $routeMiddleware = [
        ...
        'roleType' => \App\Http\Middleware\RoleType::class,
    ];
```
Step 3: Add Route <br>
Now, add route and add RoleType middleware in this route. Also, you can add middleware to the group of routes. 
```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UserController;
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('check/role',[UserController::class,'checkRole'])->middleware('roleType');
```

Step 4 :  Add Controller <br>
In this step, we will create UserController with a checkrole function.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    public function checkRole()
    {
        dd('checkRole');
    }
}
```
Now it's time to run this example copy the below link in your URL and get output.

http://localhost:8000/check/role?type=user
<br>
http://localhost:8000/check/role?type=admin 
