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
Глава 7. Модели
```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Service extends Model
{
}
```

```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Service extends Model
{
	protected $table = ‘custom_table’;
}
```

```php
class Service extends Model
{
   protected $fillable = [
       'title',
       'code'
   ];
}

```

```php
$user->create(request()->all());
```
Глава 7.1. QueryBuilder
```php
$users = User::query()->where(‘active’, true)->where(‘banned’, false)
```

```php
SELECT * FROM users WHERE active = 1 AND banned = 0;
```

```php
$user = User::query()->where(‘active’, true)->where(‘banned’, false)->first()
```

```php
$users = User::query()->where(‘active’, true)->where(‘banned’, false)->get()
```
Глава 7.2. Collections
```php
$users->sortBy(‘name’)->filter(fn($user) => $user->id > 1)
```
Глава 8. Отношения
```php
php artisan make:model Phone
```

```php
<?php
 
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Phone extends Model
{
   public function user()
   {
       return $this->belongsTo(User::class);
   }
}
```

```php
return $this->belongsTo(User::class, 'foreign_key', 'owner_key');
```

```php
return $this->belongsTo(User::class, 'telefon_uuid', 'uuid');
```

```php
$phone->user->value
```

```php
$phone->user_id = 1; 
$phone->save();

```

```php
$user = User::find(1);
$phone->user()->associate($user);
$phone->save();
```

```php
$phone->user()->save(new User([name => ‘CutCode’]))
```

```php
$phone->user()->create([‘name’ => ‘CutCode’])
```

```php
$phone->user()->saveMany([
	new User([‘name’ => ‘CutCode’]),
	new User([‘name’ => ‘Ivan’])
])

```

```php
$phone->user()->createMany([
	[name => ‘CutCode’],
[name => ‘Ivan’]
])

```

```php
$phone->user()->update([‘name’ => ‘Oleg’])
```

```php
$phone->user()->save($user)
```

```php
$phone->user()->delete()
```
8.2. HasMany - отношение “Один ко многим” 
```php
public function comments()
   {
       return $this->hasMany(Comment::class);
   }

```


```php
public function post()
   {
       return $this->belongsTo(Post::class);
   }

```

```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class User extends Model
{
   public function phone()
   {
       return $this->hasOne(Phone::class);
   }
}

```

```php
$user->phone->value
```

```php
public function comments()
   {
       return $this->hasMany(Comment::class);
   }

public function comment()
   {
       return $this->hasOne(Comment::class);
   }

```

```php
SELECT * FROM comments WHERE post_id = 1;
```

```php
SELECT * FROM comments WHERE post_id = 1 LIMIT 1;
```
8.4. Расширенное использование HasOne
```php
public function lastComment(): HasOne
{
	return $this->hasOne(Comment::class)->latestOfMany();
}

```

```php
public function firstComment(): HasOne
{
	return $this->hasOne(Comment::class)->oldestOfMany();
}

```

```php
public function firstComment(): HasOne
{
	return $this->hasOne(Comment::class)->oldestOfMany(‘identity’);
}

```

```php
public function currentPricing()
{
   return $this->hasOne(Price::class)->ofMany([
       'published_at' => 'max',
       'id' => 'max',
   ], function ($query) {
       $query->where('published_at', '<', now());
   });
}

```

```php
public function firstComment(): HasOne
{
	return $this->comments()->one()->oldestOfMany();
}

```
8.5. BelongsToMany - отношение многие ко многим 
```php
users
   id - integer
   name - string
 
roles
   id - integer
   name - string
 
role_user
   user_id - integer
   role_id - integer
```

```php
Schema::create('role_user', function (Blueprint $table) {
   $table->id();


   $table->foreignId('user_id')
       ->constrained()
       ->cascadeOnDelete()
       ->cascadeOnUpdate();


   $table->foreignId('role_id')
       ->constrained()
       ->cascadeOnDelete()
       ->cascadeOnUpdate();


   $table->timestamps();
});

```

```php
public function roles()
   {
       return $this->belongsToMany(Role::class);
   }

```

```php
Schema::create('product_property', function (Blueprint $table) {
   $table->id();


   $table->foreignId(‘product_id’)
       ->constrained()
       ->cascadeOnDelete()
       ->cascadeOnUpdate();


   $table->foreignId('property_id')
       ->constrained()
       ->cascadeOnDelete()
       ->cascadeOnUpdate();


   $table->string(‘value’);


   $table->timestamps();
});

```

```php
public function properties()
   {
       return $this->belongsToMany(Property::class)
->withPivot(‘value’);
   }

```

```php
foreach ($products->properties as $property) {
   echo $property->pivot->value;
}

```

```php
$product = Product::find(1);
$products->properties()->attach(1);
```

```php
$products->properties()->attach(1, ['value' => ‘128 mb’]);
```
```php
$product = Product::find(1);
$products->properties()->detach(1);
```

```php
$products->properties()->sync([1,2,3]);
```

```php
$products->properties()->toggle([1, 2, 3]);
```

```php
// начальное состояние
Характеристик товара []
Характеристики айди 1, 10, 20

//добавляем характеристику id 10
tovar -> toggle (10)
Характеристик товара [10]

//переключаем характеристику id 10 и добавляем характеристику id 20
tovar -> toggle (10, 20)
Характеристик товара [20]

```
```php
$products->properties()->sync([1 => ['value' => ‘128 mb’], 2, 3]);
```
8.6. HasOneThrough - отношение один к одному через таблицу 
```php
mechanics
   id - integer
   name - string
 
cars
   id - integer
   model - string
   mechanic_id - integer
 
owners
   id - integer
   name - string
   car_id - integer

```

```php
class Mechanic extends Model
{
   /**
    * Get the car's owner.
    */
   public function carOwner()
   {
       return $this->hasOneThrough(Owner::class, Car::class);
   }
}

```

```php
public function carOwner()
   {
       return $this->hasOneThrough(
           Owner::class,
           Car::class,
           'mechanic_id', // Ключ в таблице cars - связь с текущей таблицей mechanics
           'car_id', // Ключ в таблице owners связь с таблицей cars через которую мы двигаемся
           'id', // Primary key в mechanics
           'id' // Primary key в cars
       );
   }

```
8.7. HasManyThrough - отношение один ко многим через таблицу 

```php
class Mechanic extends Model
{
   /**
    * Get the car's owner.
    */
   public function carOwners()
   {
       return $this->hasManyThrough(Owner::class, Car::class);
   }
}

```

```php
class Mechanic extends Model
{
   public function carOwners()
   {
       return $this->through(‘cars’)->has(‘owners’);
   }
}

```

```php
class Mechanic extends Model
{
   public function cars()
   {
       return $this->hasMany(Car::class);
   }
}

```

```php
class Car extends Model
{
   public function owners()
   {
       return $this->hasMany(Owner::class);
   }
}

```
```php
class Car extends Model
{
   public function owner()
   {
       return $this->hasOne(Owner::class);
   }
}

```

```php
class Mechanic extends Model
{
   /**
    * Get the car's owner.
    */
   public function carOwner()
   {
       return $this->through(‘cars’)->has(‘owners’);
   }
}

```
8.8.Полиморфные отношения
```php
posts
   id - integer
   title - string
 
blogs
   id - integer
   title - string

news
   id - integer
   title - string
 
comments
   id - integer
   text - text
   commentable_id - integer
   commentable_type - string

```
```php
class Post extends Model
{
   /**
    * Get all of the post's comments.
    */
   public function comments()
   {
       return $this->morphMany(Comment::class, 'commentable');
   }
}

```

```php
​​class Comment extends Model
{
   /**
    * Get the parent commentable model (post or video).
    */
   public function commentable()
   {
       return $this->morphTo();
   }
}

```

```php
$post->comments
$blog->comments
$new->comments

```

```php
posts
   id - integer
 
blogs
   id - integer

news
   id - integer
 
images
   id - integer
   url - string
   imageable_id - integer
   imageable_type - string
```
```php
class Post extends Model
{
   /**
    * Get the post's image.
    */
   public function image()
   {
       return $this->morphOne(Image::class, 'imageable');
   }
}

```

```php
posts
   id - integer
   title - string
 
blogs
   id - integer
   title - string
 
news
   id - integer
   title - string

tags
   id - integer
   name - string
 
taggables
   tag_id - integer
   taggable_id - integer
   taggable_type - string

```

```php
class Tag extends Model
{
   /**
    * Get all of the posts that are assigned this tag.
    */
   public function posts()
   {
       return $this->morphedByMany(Post::class, 'taggable');
   }
 
   /**
    * Get all of the videos that are assigned this tag.
    */
   public function blogs()
   {
       return $this->morphedByMany(Blog::class, 'taggable');
   }
}

```

```php
class Post extends Model
{
   /**
    * Get all of the tags for the post.
    */
   public function tags()
   {
       return $this->morphToMany(Tag::class, 'taggable');
   }
}

```
8.9. Eager load
```php
foreach($post->comments as $comment) {
	echo $comment->author->name;
}

```

```php
$comments = Comment::query()->with(‘author’)->get();

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```

```php

```
