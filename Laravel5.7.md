
#Laravel5.7
##基础
##基础路由
最基本的Laravel路由接受URI和`Closure`，提供了一种非常简单且富有表现力的方法来定义路径：

```php
Route::get('foo', function () {
    return 'Hello World';
});
```
####默认路由文件
所有Laravel路由都在路径文件中定义，这些文件位于routes目录中。这些文件由框架自动加载。`routes/web.php`定义了适用于您的Web界面的路由,这些路由分配给`web`中间件组，后者提供会话状态和CSRF保护等功能。`webroutes/api.php`路由是无状态的，并被分配`api`中间件组。

对于大多数应用程序，您将首先在`routes/web.php`文件中定义路由。`routes/web.php`中定义的路由可以通过在浏览器中输入定义路由的网址进行访问。例如，您可以通过导航到浏览器来访问以下路线：http://your-app.test/user

```php
Route::get('/user', 'UserController@index');
```
`routes/api.php`文件中定义的路由嵌套在`RouteServiceProvider`路由组中。在此组中，`/api`将自动应用URI前缀，因此您无需手动将其应用于文件中的每个路由。您可以通过修改`RouteServiceProvider`类来修改前缀和其他路由组选项。

####可用路由器方法
路由器允许您注册响应任何HTTP方式的路由：

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```
有时您可能需要注册响应多个HTTP方式的路由。您可以使用`match`方法。甚至可以使用`any`方法注册响应所有HTTP方式的路由：

```php
Route::match(['get', 'post'], '/', function () {
    //
});

Route::any('foo', function () {
    //
});
```

####CSRF保护
HTML表单向`web`路由文件中定义的POST、PUT或DELETE路由提交内容时，应当包括CSRF令牌字段。否则，请求将被拒绝。您可以在CSRF文档中阅读有关[CSRF保护的更多信息](https://laravel.com/docs/5.7/csrf)：

```html
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```
####重定向路由
如果要重定向到另一个URI的路由，则可以使用`Route::redirect`方法。此方法提供了方便的快捷方式，因此您无需为执行简单重定向定义完整路由或控制器：

```php
Route::redirect('/here', '/there', 301);
```

####查看路线
如果您的路线只需要返回视图，则可以使用`Route::view`方法。与`redirect`方法一样，`view`方法提供了一个简单的快捷方式，因此您无需定义完整路径或控制器。该方法接受URI作为其第一个参数，并将视图名称作为其第二个参数。此外，您可以提供一组数据作为可选的第三个参数传递给视图：

```php
Route::view('/welcome', 'welcome');

Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
```

###路线参数

####必需参数
当然，有时您需要捕获路线中的URI段。例如，您可能需要从URL捕获用户的ID。您可以通过定义路由参数来执行此操作：

```php
Route::get('user/{id}', function ($id) {
    return 'User '.$id;
});
```
您可以根据路由的要求定义任意数量的路由参数：

```php
Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
});
```
路径参数始终包含在`{}`内，并且应包含字母字符，并且可能不包含`-`字符。使用下划线（`_`）代替`-`。路由参数根据其顺序注入到路由回调/控制器回调，控制器参数的名称无关紧要。


####可选参数
有时您可能需要指定路由参数，但可以选择存在该路由参数。您可以在参数名称后面放置一个`?`标记。确保为路由的相应变量赋予默认值：

```php
Route::get('user/{name?}', function ($name = null) {
    return $name;
});

Route::get('user/{name?}', function ($name = 'John') {
    return $name;
});
```

正则表达式约束
您可以使用where路由实例上的方法约束路由参数的格式。该where方法接受参数的名称和定义参数应如何约束的正则表达式：

Route::get('user/{name}', function ($name) {
    //
})->where('name', '[A-Za-z]+');

Route::get('user/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('user/{id}/{name}', function ($id, $name) {
    //
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);

全球约束
如果您希望路由参数始终受给定正则表达式的约束，则可以使用该pattern方法。您应该在以下boot方法中定义这些模式RouteServiceProvider：

/**
 * Define your route model bindings, pattern filters, etc.
 *
 * @return void
 */
public function boot()
{
    Route::pattern('id', '[0-9]+');

    parent::boot();
}
定义模式后，它将使用该参数名称自动应用于所有路由：

Route::get('user/{id}', function ($id) {
    // Only executed if {id} is numeric...
});

命名路由
命名路由允许方便地生成特定路由的URL或重定向。您可以通过将name方法链接到路径定义来指定路径的名称：

Route::get('user/profile', function () {
    //
})->name('profile');
您还可以为控制器操作指定路由名称：

Route::get('user/profile', 'UserProfileController@show')->name('profile');
生成指定路由的URL
为给定路径指定名称后，可以在生成URL或通过全局route函数重定向时使用路径名称：

// Generating URLs...
$url = route('profile');

// Generating Redirects...
return redirect()->route('profile');
如果命名路由定义了参数，则可以将参数作为第二个参数传递给route函数。给定的参数将自动插入URL中的正确位置：

Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');

$url = route('profile', ['id' => 1]);
检查当前路线
如果要确定当前请求是否已路由到给定的命名路由，则可以named在Route实例上使用该方法。例如，您可以从路由中间件检查当前路由名称：

/**
 * Handle an incoming request.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Closure  $next
 * @return mixed
 */
public function handle($request, Closure $next)
{
    if ($request->route()->named('profile')) {
        //
    }

    return $next($request);
}

路线组
路由组允许您跨大量路由共享路由属性，例如中间件或命名空间，而无需在每条路由上定义这些属性。共享属性以数组格式指定为方法的第一个参数。Route::group


中间件
要将中间件分配给组中的所有路由，可以middleware在定义组之前使用该方法。中间件按照它们在数组中列出的顺序执行：

Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });

    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});

命名空间
路由组的另一个常见用例是使用以下namespace方法将相同的PHP命名空间分配给一组控制器：

Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\Http\Controllers\Admin" Namespace
});
请记住，默认情况下，RouteServiceProvider在命名空间组中包含路由文件，允许您注册控制器路由，而无需指定完整的命名空间前缀。因此，您只需要指定基本命名空间之后的命名空间部分。App\Http\ControllersApp\Http\Controllers


子域路由
路由组也可用于处理子域路由。可以像路径URI一样为子域分配路由参数，允许您捕获子域的一部分以供路由或控制器使用。可以domain在定义组之前通过调用方法来指定子域：

Route::domain('{account}.myapp.com')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        //
    });
});

路线前缀
该prefix方法可以用于为组中的每个路由添加给定URI。例如，您可能希望为组内的所有路径URI添加前缀admin：

Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});

路由名称前缀
该name方法可用于为组中的每个路由名称添加给定字符串。例如，您可能希望为所有分组路径的名称添加前缀admin。给定字符串的前缀与路由名称完全一致，因此我们将确保.在前缀中提供尾随字符：

Route::name('admin.')->group(function () {
    Route::get('users', function () {
        // Route assigned name "admin.users"...
    })->name('users');
});

路线模型绑定
将模型ID注入路径或控制器操作时，通常会查询以检索与该ID对应的模型。Laravel路径模型绑定提供了一种将模型实例直接自动注入路径的便捷方法。例如，您可以注入User与给定ID匹配的整个模型实例，而不是注入用户的ID。


隐式绑定
Laravel自动解析路径或控制器操作中定义的Eloquent模型，其类型提示的变量名称与路径段名称匹配。例如：

Route::get('api/users/{user}', function (App\User $user) {
    return $user->email;
});
由于$user变量是类型提示为Eloquent模型而变量名称与URI段匹配，因此Laravel将自动注入具有与请求URI中的相应值匹配的ID的模型实例。如果在数据库中找不到匹配的模型实例，则会自动生成404 HTTP响应。App\User{user}

自定义密钥名称
如果您希望模型绑定使用除id检索给定模型类之外的数据库列，则可以覆盖getRouteKeyNameEloquent模型上的方法：

/**
 * Get the route key for the model.
 *
 * @return string
 */
public function getRouteKeyName()
{
    return 'slug';
}

显式绑定
要注册显式绑定，请使用路由器的model方法为给定参数指定类。您应该boot在RouteServiceProvider类的方法中定义显式模型绑定：

public function boot()
{
    parent::boot();

    Route::model('user', App\User::class);
}
接下来，定义包含参数的路由：{user}

Route::get('profile/{user}', function (App\User $user) {
    //
});
由于我们已将所有参数绑定到模型，因此将将实例注入到路径中。因此，例如，请求将从具有ID的数据库中注入实例。{user}App\UserUserprofile/1User1

如果在数据库中找不到匹配的模型实例，则会自动生成404 HTTP响应。

自定义分辨率逻辑
如果您希望使用自己的分辨率逻辑，则可以使用该方法。在你传递给方法，将收到的URI段的价值，应该返回应注入路由的类的实例：Route::bindClosurebind

public function boot()
{
    parent::boot();

    Route::bind('user', function ($value) {
        return App\User::where('name', $value)->first() ?? abort(404);
    });
}

后备路线
使用该方法，您可以定义在没有其他路由与传入请求匹配时将执行的路由。通常，未处理的请求将通过应用程序的异常处理程序自动呈现“404”页面。但是，由于您可以在文件中定义路由，因此midddleware组中的所有中间件都将应用于该路由。当然，您可以根据需要自由添加其他中间件到此路由：Route::fallbackfallbackroutes/web.phpweb

Route::fallback(function () {
    //
});

限速
Laravel包含一个中间件，用于限制对应用程序中路径的访问。首先，将throttle中间件分配给路由或一组路由。所述throttle中间件接受确定可以在几分钟内的给定数目进行请求的最大数目的两个参数。例如，让我们指定经过身份验证的用户每分钟可以访问以下路由组60次：

Route::middleware('auth:api', 'throttle:60,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});
动态速率限制
您可以根据经过身份验证的User模型的属性指定动态请求最大值。例如，如果User模型包含rate_limit属性，则可以将属性的名称传递给throttle中间件，以便它用于计算最大请求数：

Route::middleware('auth:api', 'throttle:rate_limit,1')->group(function () {
    Route::get('/user', function () {
        //
    });
});

表格方法欺骗
HTML表单不支持PUT，PATCH或DELETE动作。所以，当定义PUT，PATCH或DELETE路由被从HTML表单调用，你将需要隐藏的加_method场的形式。随_method字段一起发送的值将用作HTTP请求方法：

<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="{{ csrf_token() }}">
</form>
您可以使用@methodBlade指令生成_method输入：

<form action="/foo/bar" method="POST">
    @method('PUT')
    @csrf
</form>

访问当前路线
您可以使用current，currentRouteName以及currentRouteAction对方法Route门面访问有关处理传入的请求路由信息：

$route = Route::current();

$name = Route::currentRouteName();

$action = Route::currentRouteAction();
请参阅API文档，了解Route facade和Route实例的基础类，以查看所有可访问的方法。
