---
title: scrapy-安装Scrapy框架
date: 2017-07-10 12:38:59
tags: scrapy
---

## virtualenv-python环境容器

如果两个python项目分别依赖了不同版本的相同组件库，这个应该怎么管理？

这里给出了管理方案-virtualenv

先来安装一下:

``` shell
python3 -m pip install virtualenv


# 国内源可以加速安装(如果你没有翻墙，请指定国内的源)
## 先把刚才的安装删除
python3 -m pip uninstall virtualenv
## 速度很快，enjoy
python3 -m pip install -i https://pypi.douban.com/simple/ virtualenv
```
### 使用 
``` shell
## 创建虚拟环境目录
mkdir ~/envs
## 创建虚拟环境env
python3 -m virtualenv demo
## 进入bin目录
cd ~/envs/demo
## 进入虚拟环境
source ./bin/activate
## 退出虚拟环境
deactivate

## 指定初始化的python版本
python3 -m virtualenv -p /usr/bin/python2.7 python2.7-env
```

## virtualenvwrapper 更好的管理virtualenv

``` shell
# 安装
python3 -m pip install virtualenvwrapper

# 指定virtualenv目录

export WORKON_HOME=$HOME/.virtualenvs

# 查找初始化脚本
sudo find / -name virtualenvwrapper.sh

# 初始化（放到.zshrc或者.bashrc中，避免每次初始化）
source /usr/local/bin/virtualenvwrapper.sh

# 创建init环境
## 创建env
mkvirtualenv env1
mkvirtualenv env2
## 帮助
mkvirtualenv --help
## 指定python版本
mkvirtualenv --python=/usr/bin/python2.7 p27


# 退出虚拟环境
deactivate

# 列出虚拟环境
workon

# 切换到指定的环境
workon env2
```

我们可以在指定的环境下安装module，不会污染到其他的环境，如：

``` shell
## 切换到env1
workon env1

## 安装requests
pip install requests

## 测试
python

### 进入python后import测试是否安装成功
import requests

## 切换到env2
workon env2

## 测试
python

### 进入python后import测试是否安装成功
import requests
#### 提示失败!!:ModuleNotFoundError: No module named 'requests'
```
我们利用virtaulenv分离开依赖的环境，利用virtualenvwrapper来方便快捷的管理！



## Scrapy

我们需要安装最新版本的

- python3
- pip

``` shell

/usr/bin/python3 -m pip

# 如果提示：/usr/bin/python3: No module named pip

# 需要为python3安装pip
# ubuntu
sudo apt-get install python3-pip

# centos7
sudo yum install python34-setuptools
sudo easy_install pip

# 更新pip
python3 -m pip install -U pip

# 查看pip最新版本
python3 -m pip -V
# pip 9.0.1 from /usr/local/lib/python3.5/dist-packages (python 3.5)
```

## 安装Scrapy

``` shell
python3 -m pip install scrapy
```

## 测试Scrapy是否安装成功

``` shell
scrapy
# scrapy 1.4.0 - no active project

# Usage:
#  scrapy <command> [options] [args]
#
# Available commands:
#  bench         Run quick benchmark test
#  fetch         Fetch a URL using the Scrapy downloader
#  genspider     Generate new spider using pre-defined templates
#  runspider     Run a self-contained spider (without creating a project)
#  settings      Get settings values
#  shell         Interactive scraping console
#  startproject  Create new project
#  version       Print Scrapy version
#  view          Open URL in browser, as seen by Scrapy
#
#  [ more ]      More commands available when run from project directory

# Use "scrapy <command> -h" to see more info about a command
```
