# text
```php
composer create-project laravel/laravel example-app
```

```php
cd example-app
php artisan serve
```

```php
php artisan make:controller MyFirstController
```

```php
php artisan list
```

Глава 1. Прогулки по пути запроса

```php
require __DIR__.'/../vendor/autoload.php';
```

```php
$app = require_once __DIR__.'/../bootstrap/app.php';
```

```php
$app = require_once __DIR__.'/../bootstrap/app.php';
```
```php
$app->singleton(
   Illuminate\Contracts\Http\Kernel::class,
   App\Http\Kernel::class
);


$app->singleton(
   Illuminate\Contracts\Console\Kernel::class,
   App\Console\Kernel::class
);
```

```php
$kernel = $app->make(Illuminate\Contracts\Console\Kernel::class);
```

```php
$response = $kernel->handle(
   $request = Request::capture()
)->send();
```

```php
$request = Request::capture()
```

```php
$kernel->handle
```

```php
public function handle($request)
{
   try {
       $request->enableHttpMethodParameterOverride();

       $response = $this->sendRequestThroughRouter($request);
   } catch (Throwable $e) {
       $this->reportException($e);

       $response = $this->renderException($request, $e);
   }

   $this->app['events']->dispatch(
       new RequestHandled($request, $response)
   );

   return $response;
}
```

```php
$response = $this->sendRequestThroughRouter($request);
```

```php
protected function sendRequestThroughRouter($request)
{
   $this->app->instance('request', $request);

   Facade::clearResolvedInstance('request');

   $this->bootstrap();

   return (new Pipeline($this->app))
               ->send($request)
               ->through($this->app->shouldSkipMiddleware() ? [] : $this->middleware)
               ->then($this->dispatchToRouter());
}

```

```php
   return (new Pipeline($this->app))
               ->send($request)
               ->through($this->app->shouldSkipMiddleware() ? [] : $this->middleware)
               ->then($this->dispatchToRouter());
}

```

###Глава 2. Middleware

```php
public function handle($request, Closure $next)
{
   //

   return $next($request);
}

```

```php
return $next($request);
```

```php
public function handle($request, Closure $next)
{
   if ($request->ip() !== ‘190.90.90.90’) {
  	abort(404); 
  }
   return $next($request);
}

```

```php
protected $middleware = [
   // \App\Http\Middleware\TrustHosts::class,
   \App\Http\Middleware\TrustProxies::class,
   \Illuminate\Http\Middleware\HandleCors::class,
   \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
   \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
   \App\Http\Middleware\TrimStrings::class,
   \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
];

```

```php
protected $middlewareGroups = [
   'web' => [
       \App\Http\Middleware\EncryptCookies::class,
       \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
       \Illuminate\Session\Middleware\StartSession::class,
       \Illuminate\View\Middleware\ShareErrorsFromSession::class,
       \App\Http\Middleware\VerifyCsrfToken::class,
       \Illuminate\Routing\Middleware\SubstituteBindings::class,
   ],
   'api' => [
       // \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
       'throttle:api',
       \Illuminate\Routing\Middleware\SubstituteBindings::class,
   ],
];

```
###Глава 3. Route
```php
public function dispatchToRoute(Request $request)
{
   return $this->runRoute($request, $this->findRoute($request));
}

```

```php
public function boot()
{
   $this->routes(function () {
       Route::middleware('web')
           ->group(base_path('routes/web.php'));
   });
}

```

```php
'web' => [
       \App\Http\Middleware\EncryptCookies::class,
       \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
       \Illuminate\Session\Middleware\StartSession::class,
       \Illuminate\View\Middleware\ShareErrorsFromSession::class,
       \App\Http\Middleware\VerifyCsrfToken::class,
       \Illuminate\Routing\Middleware\SubstituteBindings::class,
   ],

```

```php
Route::get('/', function () {
   return view('welcome');
});

```
###Глава 4. Структура
```php
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=

```
###Глава 5. Конвенция наименований
```php

```
Глава 6. Миграции
```php
php artisan make:model User -m
```

```php
php artisan make:migration create_users_table
```

```php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateUsersTable extends Migration
{
   /**
    * Run the migrations.
    *
    * @return void
    */
   public function up()
   {
       Schema::create('users', function (Blueprint $table) {
           $table->id();
           $table->string('name');
           $table->string('email')->unique();
           $table->timestamp('email_verified_at')->nullable();
           $table->string('password');
           $table->rememberToken();
           $table->timestamps();
       });
   }

   /**
    * Reverse the migrations.
    *
    * @return void
    */
   public function down()
   {
       Schema::dropIfExists('users');
   }
}

```

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
   /**
    * Run the migrations.
    *
    * @return void
    */
   public function up()
   {
       Schema::table('news', function (Blueprint $table) {
           $table->boolean('active')->default(true);
       });
   }

   /**
    * Reverse the migrations.
    *
    * @return void
    */
   public function down()
   {
       Schema::table('news', function (Blueprint $table) {
           $table->dropColumn('active')
       });
   }
};

```

```php
php artisan migrate 
```

```php
php artisan migrate:rollback 
```

```php
php artisan migrate:rollback –step=2 
```

```php

```

```php

```

```php

```

```php

```





