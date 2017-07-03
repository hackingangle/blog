title: 3-laravel-route
date: 2016-01-26 11:31:47
tags: laravel
---

# 路由

## 一切的开始

web程序功能的最直观体现是什么？

> URL

对了，就是URL。路由就是为URL指定要执行的程序。

## 使用

### 最简单路由

想象到吗？一个最简单的闭环就可以提供路由。
``` php
Route::get('hi', function () {
    return 'Hi, World';
});
```
访问URL：
> http://localhost:8000/hi

可以获取返回结果：
> Hi, World

### 一类路由

一类路由有共同的某些特征，为了避免重复定义，'laravel'提供了此特性。

``` php
/**
 * namespace：类namespace
 * prefix：url前缀
 * middleware：自动执行动作
 */
Route::group(['namespace' => 'Admin', 'prefix' => 'admin', 'middleware' => ['web']], function () {
    Route::get('welcome', 'WelcomeController@index');
});
```

### RESTFUL

``` php
// 指定method
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);

// 快捷指定method
Route::match(['get', 'post'], '/', function () {
    //
});

// 任意匹配
Route::any('foo', function () {
    //
});
```

### 执行的特定上下文

请通过参数来传递命令

``` php
// 必选
Route::get('user/{id}', function ($id) {
    return 'User '.$id;
});
Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
});

// 可选
Route::get('user/{name?}', function ($name = null) {
    return $name;
});
Route::get('user/{name?}', function ($name = 'John') {
    return $name;
});
```

