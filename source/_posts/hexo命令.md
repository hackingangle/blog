title: hexo命令
date: 2017-07-03 22:53:47
tags: hexo 速查
---

hexo很久没用了，新学习技术，想搞个记录总结的地方，重新拾起来hexo!

怎么用呢，忘了，咋办？？？

用linux下命令重新拾起来吧，history工具大赞

``` shell
history |grep hexo

> hexo init
> hexo init blog
> hexo new page "tags"
> hexo new page categories
> hexo deploy --debug
> hexo new '1-laravel-install'
> hexo serve -w
> # 发布到git pages
> hexo clean;hexo deploy --debug
> # 创建post
> hexo new post "hexo命令"
> # 本地调试
> hexo serve -w -debug
```
