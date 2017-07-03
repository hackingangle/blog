title: 2-laravel-configuration
date: 2016-01-25 13:57:42
tags: laravel
---

# 配置

## 哪里开始?

config 文件夹，这里主要配置
``` shell
$ config [master] ⚡ ll
total 128
-rw-r--r--  1 hackingangle  staff   8.0K  1 25 14:05 app.php
-rw-r--r--  1 hackingangle  staff   2.2K 11 15 15:52 auth.php
-rw-r--r--  1 hackingangle  staff   1.4K 11 15 15:52 broadcasting.php
-rw-r--r--  1 hackingangle  staff   2.1K 11 15 15:52 cache.php
-rw-r--r--  1 hackingangle  staff   983B 11 15 15:52 compile.php
-rw-r--r--  1 hackingangle  staff   4.0K 11 15 15:52 database.php
-rw-r--r--  1 hackingangle  staff   2.5K 11 15 15:52 filesystems.php
-rw-r--r--  1 hackingangle  staff   4.3K 11 15 15:52 mail.php
-rw-r--r--  1 hackingangle  staff   2.6K 11 15 15:52 queue.php
-rw-r--r--  1 hackingangle  staff   985B 11 15 15:52 services.php
-rw-r--r--  1 hackingangle  staff   5.2K 11 15 15:52 session.php
-rw-r--r--  1 hackingangle  staff   1.0K 11 15 15:52 view.php
```

- app
    - web程序的公共，service注册，时区设置，语言设定，debug模式开关，加密随机串，加密算法，别名，日志
- database
    - 数据库:mysql,sqlite,pgsql,sqlsrv,redis
    - 返回类型:array|object
    - migrations路径

## 访问配置，用起来吧
``` php
// 获取时区
$value = config('app.timezone');

// 运行时，内存级改变
config([
    'app.timezone' => 'UTC',
]);
```

## 让敏感信息躲在暗处

laravel 当前版本支持把数据库连接信息，加密随机串等敏感信息隐秘起来。所谓的隐秘，就是不让这些信息走版本控制。

### env 来干活

``` php
env('DB_CONNECTION', 'mysql');
```
上面的话被引用自`config/database.php`用来从`.env文件`中读取DB_CONNECTION。版本控制中隐藏掉`.env文件`。

### 运行环境

运行环境概念，允许区分线上、线下以及一些特殊的环境下的不同配置。

``` php
// App
$environment = App::environment();
// instance
$environment = app()->environment();

// 判断
if (App::environment('local')) {
    // 环境为local执行
}
if (App::environment('local', 'staging')) {
    // 环境为local 或 staging执行
}
```

## 线上优化

合并配置文件，被framework快速加载

``` shell
php artisan config:cache
```
## 维护模式

维护模式重定向到resources/views/errors/503.blade.php

``` shell
# 关闭维护模式
php artisan up
# 开启维护模式
php artisan down
```
