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
