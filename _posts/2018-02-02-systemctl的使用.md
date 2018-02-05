---

layout: post
title: systemctl的使用
categories: Linux
description: systemctl的使用
keywords: Linux
--------------------------------------------------------------------------------------------

在项目中, 总会用到各种的服务. 如果这个服务没有封装, 我一般都会用`nohug`来后台运行.
直到前几天查看一些资料, 发现可以自己写一个配置文件,
就可以使用`systemctl`来控制服务的运行了. 所以, 我又有事干了.

## 参考

[Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

[Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

[systemd.service 中文手册](http://www.jinbuguo.com/systemd/systemd.service.html)

## 总结

传统的启动一个服务. 我们可以使用 `nohug` 等命令来实现 `daemon`, 现在可以用
`systemctl` 来实现了. 而且也更容易查看状态等信息.

为了测试. 我特意的在 `centos7` 虚拟机里安装了 `redis` 服务端. 来模拟实战.

安装步骤略过. so easy. 你肯定会.

### 常用命令

```
# 列出正在运行的 Unit
$ systemctl list-units

# 列出所有Unit，包括没有找到配置文件的或者启动失败的
$ systemctl list-units --all

# 列出所有没有运行的 Unit
$ systemctl list-units --all --state=inactive

# 列出所有加载失败的 Unit
$ systemctl list-units --failed

# 列出所有正在运行的、类型为 service 的 Unit
$ systemctl list-units --type=service
```

例如我们忘了自定义服务的全部名称, 只记得一部分. 可以这么查询:

```
[root@CentOS7 redis-4.0.8]# systemctl list-unit-files --type=service | grep console
console-getty.service                         disabled
console-shell.service                         disabled
systemd-ask-password-console.service          static
systemd-vconsole-setup.service                static
[root@CentOS7 redis-4.0.8]#
```

### 自定义服务

#### 添加配置文件

在 `/etc/systemd/system`下创建文件. 必须以 `.service` 作为后缀

```
[root@CentOS7 system]# vim /etc/systemd/system/redis.service
```

```
[Unit]
Description=Redis server daemon

[Service]
Type=simple
PIDFile=/run/redis.pid
ExecStart=/usr/local/bin/redis-server
ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID  
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

#### 查看服务状态

配置完毕以后. 查看一下.

```
[root@CentOS7 system]# systemctl status redis.service
● redis.service - Redis server daemon
   Loaded: loaded (/etc/systemd/system/redis.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
[root@CentOS7 system]#
```

> `Loaded` 这一行, `disabled;` 代表不是开机启动.

#### 设置开机启动

```
[root@CentOS7 bin]# systemctl enable redis.service
Created symlink from /etc/systemd/system/multi-user.target.wants/redis.service to /etc/systemd/system/redis.service.
[root@CentOS7 bin]#
```

上面的命令相当于在 `/etc/systemd/system` 目录添加一个符号链接，指向
`/usr/lib/systemd/system` 里面的 `redis.service` 文件。

这是因为开机时，Systemd只执行 `/etc/systemd/system`
目录里面的配置文件。这也意味着，如果把修改后的配置文件放在该目录，就可以达到覆盖原始配置的效果。

再次查看

```
[root@CentOS7 system]# systemctl status redis.service
● redis.service - Redis server daemon
   Loaded: loaded (/etc/systemd/system/redis.service; enabled; vendor preset: disabled)
   Active: inactive (dead)
[root@CentOS7 system]#

```

可以看到. `Loaded` 中, 已经变为了 `enabled;`.

#### 运行自定义服务

```
[root@CentOS7 system]# systemctl start redis.service
[root@CentOS7 system]# systemctl stop redis.service
[root@CentOS7 system]# systemctl restart redis.service
```

#### 修改了服务

如果修改了, 要执行一下命令.

```
systemctl daemon-reload
```

### 比较

| 任务              | 旧指令                         | 新指令                                                                                            |
|:-----------------|:------------------------------|:-------------------------------------------------------------------------------------------------|
| 任务              | 旧指令                         | 新指令                                                                                            |
| 使某服务自动启动    | chkconfig --level 3 httpd on  | systemctl enable httpd.service                                                                   |
| 使某服务不自动启动   | chkconfig --level 3 httpd off | systemctl disable httpd.service                                                                  |
| 检查服务状态       | service httpd status          | systemctl status httpd.service （服务详细信息） systemctl is-active httpd.service （仅显示是否 Active) |
| 显示所有已启动的服务 | chkconfig --list              | systemctl list-units --type=service                                                              |
| 启动某服务         | service httpd start           | systemctl start httpd.service                                                                    |
| 停止某服务         | service httpd stop            | systemctl stop httpd.service                                                                     |
| 重启某服务         | service httpd restart         | systemctl restart httpd.service                                                                  |

> create_time 2018-02-05 22:43:06

