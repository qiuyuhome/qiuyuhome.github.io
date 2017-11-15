---
layout: post
title: 查看nginx运行的时长
categories: Linux
description: 查看nginx运行的时长
keywords: nginx, Linux, 查看nginx运行的时长
---

```
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

