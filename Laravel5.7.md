#  Laravel5.7
##  基础
###  基本路由
基本路由是接收一个URL和Closure，如下一个简章的示例：

```php
Route::get('foo', function () {
    return 'Hello World';
});
```

####  默认路由文件
所有的Laravel路由都被定义在位于`routes`目录下的路由文件中，这些文件将由框架自动加载,routes/web.php
