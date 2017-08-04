---
title: shell命令查看linux-tcp
date: 2017-07-13 16:02:58
tags: linux
---

## tcp

### 查看timeout数量

``` shell
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```

显示结果
``` shell
SYN_RECV 1
CLOSE_WAIT 1
ESTABLISHED 122
FIN_WAIT2 3
TIME_WAIT 55745
```
