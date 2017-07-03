title: php7安装
date: 2016-09-04 16:43:42
tags: php
---

走入php开发的世界，我们先要下载一个`php解析器`，尽情的站在巨人的肩膀上吧！

我们假设的开发环境是OSX系统，做php开发有个[macbook](http://www.apple.com/cn/macbook-pro/)真的很好用。快来入坑！

废话少说，开始搭建环境吧。准备：
1. homebrew
1. php7

## homebrew

面向OSX下的包管理工具。

### 安装

``` shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 使用
- 已安装列表
    - brew list
- 搜索
    - brew search XXX
- 安装
    - brew install XXX
- 删除
    - brew uninstall XXX

Log::
``` shell
☁  hackingangle.github.io [master] ⚡ brew
Example usage:
  brew search [TEXT|/REGEX/]
  brew (info|home|options) [FORMULA...]
  brew install FORMULA...
  brew update
  brew upgrade [FORMULA...]
  brew uninstall FORMULA...
  brew list [FORMULA...]

Troubleshooting:
  brew config
  brew doctor
  brew install -vd FORMULA

Brewing:
  brew create [URL [--no-fetch]]
  brew edit [FORMULA...]
  https://github.com/Homebrew/brew/blob/master/share/doc/homebrew/Formula-Cookbook.md

Further help:
  man brew
  brew help [COMMAND]
  brew home
```

## php7

### 安装php7

``` shell
# 搜索线上包
brew search php7

## 根据搜索结果继续安装
brew install homebrew/php/php70
```
LOG
``` shell
==> Installing php70 from homebrew/php
==> Installing dependencies for homebrew/php/php70: freetype
==> Installing homebrew/php/php70 dependency: freetype
==> Downloading https://homebrew.bintray.com/bottles/freetype-2.6.5.el_capitan.bottle.ta
######################################################################## 100.0%
==> Pouring freetype-2.6.5.el_capitan.bottle.tar.gz
```

### 使用php7

``` shell
☁  hackingangle.github.io [master] ⚡ php -v
PHP 7.0.9 (cli) (built: Jul 21 2016 14:50:47) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
```
