---
layout: post
title: 搭建svn服务器
categories: Tools
description: 搭建svn服务器
keywords: svn
---

svn, 我使用的第一款版本控制软件. 现在都是git的天下了.

首先进入工作目录, 新建个文件夹, 用来存放svn.

```bash
cd /data/
mkdir /data/svn
```

创建SVN版本库, 这里以blog为例

```bash
svnadmin create /data/svn/blog
```

创建完毕以后, 进入blog目录, 会发现里面已经自动生成了svn的目录, 如下

```bash
drwxr-xr-x 2 root root 4096 Apr 21 15:53 conf
drwxr-sr-x 6 root root 4096 Apr 21 16:32 db
-r--r--r-- 1 root root    2 Apr 21 15:31 format
drwxr-xr-x 2 root root 4096 Apr 21 15:31 hooks
drwxr-xr-x 2 root root 4096 Apr 21 15:31 locks
-rw-r--r-- 1 root root  229 Apr 21 15:31 README.txt
```

其中, conf是配置文件的目录, 进入, 是这样的内容

```bash
-rw-r--r-- 1 root root 1137 Apr 21 15:46 authz
-rw-r--r-- 1 root root  333 Apr 21 15:47 passwd
-rw-r--r-- 1 root root 2250 Apr 21 15:53 svnserve.conf
```

authz是分配权限的配置文件
passwd是用户名和密码的配置文件
svnserve.conf是配置svn服务的配置文件
3个文件都需要正确配置, 才能正确的运行svn

编辑authz文件, 改成下面的内容:

```vim
[aliases]
# joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average

# [groups], 这个就是定义组成员
[groups]
# admin是组名称, qiuy是成员名称
# harry_and_sally = harry,sally
# harry_sally_and_joe = harry,sally,&joe
admin = qiuy
test = test

# 这个是定义组的权限, r代表读, w代表写. 
# [blog:/], 代表是blog下的跟目录
# 给组定义权限, 组名前面必须有@符号
# 结合起来的意思就是, 在blog下的跟目录, admin组的所有成员有读写权限, test组只有读权限.
[blog:/]
@admin = rw
@test = r

# [/foo/bar]
# harry = rw
# &joe = r
# * =

# [repository:/baz/fuz]
# @harry_and_sally = rw
# * = r
```

编辑passwd文件, 改成下面的内容:

```vim
### This file is an example password file for svnserve.
### Its format is similar to that of svnserve.conf. As shown in the
### example below it contains one section labelled [users].
### The name and password for each user follow, one account per line.

# 定义成员, =前面的是成员名称, =后面的是成员的密码
[users]
# harry = harryssecret
# sally = sallyssecret
qiuy = qiuy
test = test
```

编辑svnserve.conf文件, 改成下面的内容:

```vim
### This file controls the configuration of the svnserve daemon, if you
### use it to allow access to this repository.  (If you only allow
### access through http: and/or file: URLs, then this file is
### irrelevant.)

### Visit http://subversion.tigris.org/ for more information.

[general]
### These options control access to the repository for unauthenticated
### and authenticated users.  Valid values are "write", "read",
### and "none".  The sample settings below are the defaults.
# 代表匿名访问svn, 的权限. 默认是read, 改成none代表无权限, 这样安全有保证
anon-access = none
# 代表认证的成员拥有的权限,write代表读写权限
auth-access = write
### The password-db option controls the location of the password
### database file.  Unless you specify a path starting with a /,
### the file's location is relative to the directory containing
### this configuration file.
### If SASL is enabled (see below), this file will NOT be used.
### Uncomment the line below to use the default password file.
# 是指密码配置文件的位置, 默认的不用修改, 除非你把密码配置文件放到了别的目录.
password-db = passwd
### The authz-db option controls the location of the authorization
### rules for path-based access control.  Unless you specify a path
### starting with a /, the file's location is relative to the the
### directory containing this file.  If you don't specify an
### authz-db, no path-based access control is done.
### Uncomment the line below to use the default authorization file.
# 是指权限配置文件的位置, 默认的不用修改, 除非你把权限配置文件放到了别的目录.
authz-db = authz
### This option specifies the authentication realm of the repository.
### If two repositories have the same authentication realm, they should
### have the same password database, and vice versa.  The default realm
### is repository's uuid.
# UUID是什么鬼
realm = /

[sasl]
### This option specifies whether you want to use the Cyrus SASL
### library for authentication. Default is false.
### This section will be ignored if svnserve is not built with Cyrus
### SASL support; to check, run 'svnserve --version' and look for a line
### reading 'Cyrus SASL authentication is available.'
#use-sasl = true
### These options specify the desired strength of the security layer
### that you want SASL to provide. 0 means no encryption, 1 means
### integrity-checking only, values larger than 1 are correlated
### to the effective key length for encryption (e.g. 128 means 128-bit
### encryption). The values below are the defaults.
# min-encryption = 0
# max-encryption = 256
```

OK, 现在配置文件都已将搞定了. 但是还没弄完, 需要启动SVN的服务.
先执行一个命令, 看看SVN有没有在运行

```bash
[root@iZ28cww0nf3Z conf]# ps aux | grep svn
root      1399  0.0  0.0 103252   840 pts/0    S+   16:58   0:00 grep svn
root     25623  0.0  0.3 143668  3460 tty1     T    Mar21   0:00 vim svnserve.conf
```

没有看到, 证明现在没有运行SVN, (第一次安装当然不会有)
那么, 执行下面的命令, 让SVN在后台做为守护进程运行.

```bash
svnserve -d -r /data/svn/
```
这里需要注意: 最后的/一定要写, 在使用SVN的时候, 会提示找不到版本库.

这个时候, 再运行命令查看SVN是否已经启动服务

```bash
[root@iZ28cww0nf3Z conf]# ps aux | grep svn
root      1418  0.0  0.0 156996   888 ?        Ss   17:02   0:00 svnserve -d -r /data/svn/
root      1422  0.0  0.0 103252   840 pts/0    S+   17:04   0:00 grep svn
root     25623  0.0  0.3 143668  3460 tty1     T    Mar21   0:00 vim svnserve.conf
```

看到了, 第一样就是我们之前运行的让SVN启动为守护进程的命令, 证明已经成功了. 

还没有结束, 如何导出文件呢?
进入你想要导出的目录, 例如:

```bash
[root@iZ28cww0nf3Z www]# cd /data/www
[root@iZ28cww0nf3Z www]# mkdir blog
[root@iZ28cww0nf3Z essay]# svn checkout svn://127.0.0.1/essay
```


这个时候会要求你输入用户名和密码, 就是配置SVN密码文件时候设置的用户名和密码

```bash
Authentication realm: <svn://127.0.0.1:3690> /
Password for 'root': 
Authentication realm: <svn://127.0.0.1:3690> /
Username: qiuy
Password for 'qiuy': 
```

如果输入正确, 又会出现下面的提示

```bash
    -----------------------------------------------------------------------
    ATTENTION!  Your password for authentication realm:

       <svn://127.0.0.1:3690> /

    can only be stored to disk unencrypted!  You are advised to configure
    your system so that Subversion can store passwords encrypted, if
    possible.  See the documentation for details.

    You can avoid future appearances of this warning by setting the value
    of the 'store-plaintext-passwords' option to either 'yes' or 'no' in
    '/root/.subversion/servers'.
    -----------------------------------------------------------------------
    Store password unencrypted (yes/no)? 
```

意思是, 问你是否加密密码, yes就可以了
然后就会自动导入文件到你的目录

参考资料: 
http://blog.sina.com.cn/s/blog_963453200101eiuq.html





