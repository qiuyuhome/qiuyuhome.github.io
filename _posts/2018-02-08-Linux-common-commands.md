---
layout: post
title: Linux常用的复杂命令
categories: [Linux]
description: Linux常用的复杂命令
keywords: Linux
---

(未完结) 简单的 Linux 命令大家都会, 这里记录一些复杂的, 但是又很常用的命令. 用到了我就记录下来. 也方便日后的查看.

* `nohup java -jar selenium-server-standalone-3.3.1.jar -port 10003 > ./data/seleniumLog/10001.log 2>&1 &`

    后台启动进程. 输入输出指定到文件中.

* ps -eo pid,lstart,etime,cmd | grep nginx

    查看 `linux` 进程的运行时常.

    ```bash
    [www@ns545192 shop]$ ps -eo pid,lstart,etime,cmd | grep nginx
     2244 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: master process /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
     2281 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2282 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2283 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2285 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2286 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2288 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2290 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
     2291 Fri Oct 27 10:33:18 2017 19-06:04:40 nginx: worker process
    10211 Wed Nov 15 16:37:58 2017       00:00 grep --color=auto nginx
    [www@ns545192 shop]$
    ```
