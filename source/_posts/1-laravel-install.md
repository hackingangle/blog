title: 1-laravel-install
date: 2016-01-24 17:36:57
tags: laravel
---

# 安装

## laravel安装器

通过打包安装，速度快点
``` bash
composer global require "laravel/installer"
laravel new demo
```

## composer安装

第一次较慢，而后会命中`cache`
``` bash
composer create-project --prefer-dist laravel/laravel demo
```

# 跑起来

## php内置服务器启动(php5.4+)

artisan启动
``` php
php artisan server
```

## 访问入口

http://localhost:8000/
