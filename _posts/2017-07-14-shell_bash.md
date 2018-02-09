---
layout: post
title: shell_script
categories: Linux
description: shell_script
keywords: shell_script
---

简单记录shell, 现在用的很少了. 一般都用python.

```shell
接收来自命令行传入的参数，第一个参数用$1表示，第二个参数$2表示。以此类推。
注意：$0表示脚本文件名。
另外一个在shell编程中经常用到的是“$@”这个代表所有的参数。你可以用一个循环来遍历这个参数
#!/bin/bash
for args in $@
do
        echo $args
done
```


```shell
$0	当前脚本的文件名
$n	传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。
$#	传递给脚本或函数的参数个数。
$*	传递给脚本或函数的所有参数。
$@	传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。
$?	上个命令的退出状态，或函数的返回值。
$$	当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。
```

