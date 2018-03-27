---
layout: post
title: workerman学习笔记
categories: PHP
description: workerman学习笔记
keywords: PHP, WorkerMan
---

WorkerMan 是一个纯 PHP 写的 Socket 通讯框架. 学习这个再学习 Swoole, 应该就容易多了. 这里记录了查看 WorkerMan 源码遇到的东西.

## (WorkerMan 官网)[http://www.workerman.net/]

## 遇到的不常见的 php 函数

- spl_object_hash()
- stream_context_create()
- sys_get_temp_dir()
- debug_backtrace()
- php_sapi_name()
- cli_set_process_title()
- pcntl_signal()
- posix_getpid()
- posix_kill($pid, 0);  // $pid 存在, 返回 true, 否则返回 false. 用于查看 pid 是否有效.
- umask()
- lcfirst()
- explode() 第三个参数.
- var_export()
- stream_socket_server()
- socket_import_stream()
- socket_set_option()
- stream_set_blocking()
- pcntl_fork()
- call_user_func()
- pcntl_signal_dispatch()
- pcntl_wait()

## 遇到的计算机术语

- backlog
- alarm, sigalrm
- daemonize
- 
