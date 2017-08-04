title: exec非阻塞挂起php进程
date: 2017-08-04 11:51:41
tags: [知识点, php]
categories: php
---
使用exec在一个http服务中非阻塞启动后端程序
<!-- more -->
> 问：如何通过请求一个服务，使得服务器在后端挂起一个php脚本？

> 回答：exec('xxx);

接下来，我们在这里说下xxx包含什么内容？

是否可以通过在shell中的命令来挂起？

比如：

`nohup php delay60s.php &`

执行后发现不可以！

必须要重定向输出流

`nohup php delay60s.php >> /dev/null &`

或者

`nohup php delay60s.php >> ./delay60s.log &`
