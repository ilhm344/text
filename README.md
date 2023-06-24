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
$post = Post::query()->with(‘comments.author’)->where(‘id’, 1)->first();
```

```php
$post->load(‘comments.author’);
```
8.10. QueryBuilder для отношений
```php
$posts = Post::has('comments')->get();

```

```php
$posts = Post::whereHas('comments', function (Builder $query) {
   $query->where('content', 'like', 'code%');
})->get();
```

```php
$posts = Post::whereRelation('comments', 'is_approved', false)->get();

```
8.11. Агрегатные функции для отношений
```php
$posts = Post::withCount('comments')->get();

```

```php
foreach ($posts as $post) {
   echo $post->comments_count;
}

```

```php
$posts = Post::withMax('comments', 'likes')->get();
foreach ($posts as $post) {
   echo $post->comments_max_likes;
}

```

```php
$posts = Post::withAvg('comments', 'votes')->get();
foreach ($posts as $post) {
   echo $post->comments_avg_votes;
}

$posts = Post::withSum('comments', 'votes')->get();
foreach ($posts as $post) {
   echo $post->comments_sum_votes;
}

```
9.1. Mutators/Accessors
```php
class User extends Model
{
   /**
    * Get the user's first name.
    *
    * @return \Illuminate\Database\Eloquent\Casts\Attribute
    */
   protected function phone(): Attribute
   {
       return Attribute::make(
           get: fn ($value) => ‘+’ . $value,
       );
   }
}

```

```php
class User extends Model
{
   /**
    * Get the user's first name.
    *
    * @return \Illuminate\Database\Eloquent\Casts\Attribute
    */
   protected function name(): Attribute
   {
       return Attribute::make(
           get: fn () => $this->first_name . “ ” . $this->last_name,
       );
   }
}

```

```php
class User extends Model
{
   /**
    * Get the user's first name.
    *
    * @return \Illuminate\Database\Eloquent\Casts\Attribute
    */
   protected function phone(): Attribute
   {
       return Attribute::make(
           set: fn ($value) => trim(preg_replace('/^1|\D/', "", $value)),
       );
   }
}

```

```php
return Attribute::make(
		get: fn ($value) => ‘+’ . $value,
           set: fn ($value) => trim(preg_replace('/^1|\D/', "", $value)),
       );

```
9.2. Casts
```php
class User extends Model
{
   /**
    * The attributes that should be cast.
    *
    * @var array
    */
   protected $casts = [
       'is_admin' => 'boolean',
   ];
}

```

```php
class User extends Model
{
   /**
    * The attributes that should be cast.
    *
    * @var array
    */
   protected $casts = [
       'images' => 'collection',
   ];
}

```

```php
php artisan make:cast PhoneCast
```

```php
namespace App\Casts;
use Illuminate\Contracts\Database\Eloquent\CastsAttributes;
class PhoneCast implements CastsAttributes
{
   /**
    * Cast the given value.
    *
    * @param  \Illuminate\Database\Eloquent\Model  $model
    * @param  string  $key
    * @param  mixed  $value
    * @param  array  $attributes
    * @return array
    */
   public function get($model, $key, $value, $attributes)
   {
       return ‘+’ . $value;
   }
 
   /**
    * Prepare the given value for storage.
    *
    * @param  \Illuminate\Database\Eloquent\Model  $model
    * @param  string  $key
    * @param  array  $value
    * @param  array  $attributes
    * @return string
    */
   public function set($model, $key, $value, $attributes)
   {
       return trim(preg_replace('/^1|\D/', "", $value));
   }
}

```

```php
use App\Casts\PhoneCast;
class User extends Model
{
   /**
    * The attributes that should be cast.
    *
    * @var array
    */
   protected $casts = [
       'phone' => PhoneCast::class,
   ];
}

```
Глава 10. Scopes
```php
$posts = Post::query()->where(‘active’, true)->where(‘is_moderated’, true)->where(‘banned’, false)->get()

```

```php
$posts = Post::query()->active()->get()
```

```php
public function activeScope(Builder $query)
{
	$query->where(‘active’, true)->where(‘is_moderated’, true)->where(‘banned’, false);
}

```
11.1 View
```php
Route::get('/’', function(){ 
return view('home’');
});

```

```php
Route::get('/’', function(){ 
return view('pages.home’');
});

```

```php
return view('home’', ['posts' => Post::query()->active()->get()]);

```

```php
<html>
<head>
   <title>Cutcode here!</title>
</head>
<body>
<ul>
@foreach($posts as $post)
<li>{{  $post->name }}</li>
@endforeach
</ul>
</body>
</html>

```

```php
@if(true)
@endif 

@auth 
@endauth 

```
Глава 12. Контроллер

```php
php artisan make:controller HomeController
```

```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class HomeController extends Controller
{
   //
}

```

```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class HomeController extends Controller
{
   public function index()
	{
		return view(‘home’, [‘posts’ => Post::query()->active()->get()])
}
}

```

```php
Route::get('/’', [HomeController:class, ‘index’]);

```
Глава 13. Service Container
```php
public function indexPage(Request $request)
{
}

```

```php
public function indexPage(User $user)
{
}

```

```php
class Car {
	public function __construct(
       protected string $color
   	) {}

}

```

```php
public function indexPage(Car $car)
{
}

```

```php
public function boot(): void
{
   $this->app->instance(Car::class, new Car(‘white’));
}

```

```php
$app->singleton(
   Illuminate\Contracts\Http\Kernel::class,
   App\Http\Kernel::class
);

```

```php
public function indexPage(Illuminate\Contracts\Http\Kernel $kernel)
{
}

```

```php
public function boot(): void
{
   $this->app->bind(MessengersInterface::class, Telegram::class);
}

```

```php
public function indexPage(MessengersInterface $messenger)
{
}

```

```php
   $this->app->bind(MessengersInterface::class, Slack::class);

```

```php
public function indexPage()
{
	$messenger = app(MessengersInterface::class);
}

```
Глава 13. Два брата Request и Response
```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
class HomeController extends Controller
{
   public function index(Request $request)
	{
		dump($request->all());
		// все параметры реквеста
                // получим метод запроса GET или POST PUT DELETE и остальные	
		dump($request->method());
                // Читаем куку	
		dump($request->cookie(‘name’));
                // Берем файл	
		dump($request->file(‘file’));
                // Заголовок	
		dump($request->header(‘name’));
		return view(‘home’, [‘posts’ => Post::query()->active()->get()])
}
}

```

```php
return redirect(‘/’);
```

```php
return response()->json([‘data’ => ‘’]);
```
Глава 14. Валидация
```php
<?php


namespace App\Http\Controllers;


use Illuminate\Http\Request;


class HomeController extends Controller
{
   public function index(Request $request)
	{
	      $validatedData = $request->validate([
              'title' => ['required', 'unique:posts', 'max:255'],
              'body' => ['required'],
         ]);
        }
}

```

```php
<input type=”text” name=”title” />
@error(‘title’)
{{ $message }}
@enderror

```

```php
php artisan make:request ContactForm

```

```php
<?php
namespace App\Http\Requests;
use Illuminate\Foundation\Http\FormRequest;
class ContactForm extends FormRequest
{
   /**
    * Determine if the user is authorized to make this request.
    *
    * @return bool
    */
   public function authorize()
   {
       return true;
   }

   /**
    * Get the validation rules that apply to the request.
    *
    * @return array<string, mixed>
    */
   public function rules()
   {
       return [
           'title' => ['required', 'unique:posts', 'max:255'],
   'body' => ['required'],
       ];
   }
}

```

```php
<?php
namespace App\Http\Controllers;
use App\Http\Request\ContactForm;
class HomeController extends Controller
{
   public function index(ContactForm $request)
	{}
}

```
Глава 15. Безопасность
```php
$user[‘about’] = $_POST['about'];
$query = UPDATE users SET about  = ‘$user[‘about’]’;

```

```php
“test” AND is_admin = “1’” 
```

```php
$query = UPDATE users SET about  = ‘$user[‘about’]’;
// UPDATE users SET about  = ‘test’ AND is_admin = 1
```

```php
<script>alert('hello, I just hacked this page');</script>

```

```php
{{ $user->about }}
```

```php
{!! $user->about !!} 
```

```php
<form action="https://your-application.com/user/email" method="POST">
   <input type="email" value="malicious-email@example.com">
</form>

```

```php
<form method="POST" action="/profile">
   <!-- Выведет hidden input с токеном как и пример ниже ... -->
   @csrf
 
   <!-- Тоже самое с помощью токена ... -->
   <input type="hidden" name="_token" value="{{ csrf_token() }}" />
</form>

```

```php
session()->put(‘name’, ‘value’); // записали значение в сессию с ключом name
session()->get(‘name’); // получили значение value
if(session()->has(‘name’)) {} // проверили есть ли в сессиях такие данные 

```

```php
if(auth()->attempt([‘email’ => request(‘email’), ‘password’ => request(‘password’)])) {
}

```

```php
User::query()->create([
	‘email’ => request(‘email’),
	‘password’ => Hash::make(‘password’)
])

```

```php
auth()->logout()

```

```php
auth()->login($user)

```

```php
Auth::loginUsingId(1);
```

```php
Route::get(‘/profile’, ProfileController::class)->middleware(‘auth’);

```

```php
Route::get(‘/profile’, ProfileController::class)->middleware(‘guest’);
```

```php
@auth 
Я {{ auth()->user()->name }} и мой id - {{ auth()->id() }}
@endauth

@guest 
А здесь будет кнопка “Войти”
@endguest

```

```php
php artisan make:controller HomeController
```

```php
php artisan make:cast PhoneCast
php artisan make:request ContactFormRequest

```

```php
php artisan serve
```

```php
php artisan migrate
```

```php
php artisan make:command CreateTestUse
```

```php
<?php
namespace App\Console\Commands;
use Illuminate\Console\Command;
class CreateTestUser extends Command
{
   /**
    * The name and signature of the console command.
    *
    * @var string
    */
   protected $signature = 'command:name';

	/**
    * The console command description.
    *
    * @var string
    */
   protected $description = 'Command description';

   /**
    * Execute the console command.
    *
    * @return int
    */
   public function handle()
   {
       return Command::SUCCESS;
   }
}

```

```php
php artisan command:name 
```

```php
<?php


namespace App\Console\Commands;


use Illuminate\Console\Command;


class CreateTestUser extends Command
{
   /**
    * The name and signature of the console command.
    *
    * @var string
    */
   protected $signature = 'create:user';

   /**
    * The console command description.
    *
    * @var string
    */
   protected $description = 'Create test user';

   /**
    * Execute the console command.
    *
    * @return int
    */
   public function handle()
   {
	User::create([
	‘email’ => ‘test@example.com’
        ]);
       return Command::SUCCESS;
   }
}

```

```php
php artisan create:user
```
Глава 19. Сид и нэнси? Почти! Сиды и Фабрики
```php
namespace Database\Seeders;

// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
   /**
    * Seed the application's database.
    *
    * @return void
    */
   public function run()
   {
       // \App\Models\User::factory(10)->create();
   }
}

```

```php
class DatabaseSeeder extends Seeder
{
   /**
    * Seed the application's database.
    *
    * @return void
    */
   public function run()
   {
       OrderStatus::query()->create([‘name’ => ‘new’]);
   }
}

```

```php
php artisan migrate –seed
```

```php
php artisan db:seed
```

```php
php artisan make:seeder OrderStatusSeeder

```

```php
<?php


namespace Database\Seeders;


use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;


class OrderStatusSeeder extends Seeder
{
   /**
    * Run the database seeds.
    *
    * @return void
    */
   public function run()
   {
       DB::table(‘order_statuses’)->insert([‘id’ => 1, ‘name’ => ‘Новый’]);
       DB::table(‘order_statuses’)->insert([‘id’ => 2, ‘name’ => ‘В обработке’]);
       DB::table(‘order_statuses’)->insert([‘id’ => 3, ‘name’ => ‘Подтвержден’]);
       DB::table(‘order_statuses’)->insert([‘id’ => 4, ‘name’ => ‘Оплачен’]);
   }
}

```

```php
public function run()
{
   $this->call([
       OrderStatusSeeder::class,
   ]);
}

```

```php
php artisan db:seed --class=OrderStatusSeeder
```

```php
php artisan migrate —-seed --seeder=OrderStatusSeeder
```

```php
<?php
namespace Database\Factories;
use Illuminate\Database\Eloquent\Factories\Factory;
use Illuminate\Support\Str;

/**
* @extends \Illuminate\Database\Eloquent\Factories\Factory<\App\Models\User>
*/
class UserFactory extends Factory
{
   /**
    * Define the model's default state.
    *
    * @return array<string, mixed>
    */
   public function definition()
   {
       return [
           'name' => fake()->name(),
           'email' => fake()->unique()->safeEmail(),
           'email_verified_at' => now(),
           'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
           'remember_token' => Str::random(10),
       ];
   }

   /**
    * Indicate that the model's email address should be unverified.
    *
    * @return static
    */
   public function unverified()
   {
       return $this->state(fn (array $attributes) => [
           'email_verified_at' => null,
       ]);
   }
}

```

```php
<?php
namespace Database\Seeders;
// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
   /**
    * Seed the application's database.
    *
    * @return void
    */
   public function run()
   {
       \App\Models\User::factory(100)->create();
   }
}

```

```php
php artisan db:seed
```
Глава 20. Тесты
```php
php artisan make:test ArticlesTest
```

```php
<?php
namespace Tests\Feature;
use Illuminate\Foundation\Testing\RefreshDatabase; use Illuminate\Foundation\Testing\WithFaker;
use Tests\TestCase;
class ArticlesTest extends TestCase {
   /**
     * A basic feature test example.
     *
     * @return void
     */

public function testExample() {
       $response = $this->get('/');
        $response->assertStatus(200);

} }
php artisan test --filter ArticlestTest
```

```php
OK (1 test, 1 assertion)
```

```php
public function testArticlesPage(){ 
$this->get('/articles)
          ->assertSee('All articles');
}

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
