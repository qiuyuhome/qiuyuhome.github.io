


# laravel中的几个重要概念

## 环境配置

### 参考

[laravel文档 - Laravel 的配置信息](https://d.laravel-china.org/docs/5.5/configuration)

[vlucas/phpdotenv](https://github.com/vlucas/phpdotenv)

[12-factors](https://www.12factor.net/zh_cn/)

[12-factors 在环境中存储配置](https://www.12factor.net/zh_cn/config)

> 12-Factor推荐将应用的配置存储于 环境变量 中（ env vars, env ）。
> 环境变量可以非常方便地在不同的部署间做修改，却不动一行代码；与配置文件不同，不小心把它们签入代码库的概率微乎其微；
> 与一些传统的解决配置问题的机制（比如 Java 的属性配置文件）相比，环境变量与语言和系统无关。


大家都知道，在之前的版本 Laravel 是使用 config 文件夹下的 php 文件来完成项目所需要的配置的，后面从大概从 5 开始就使用了 .env 来放置部分配置。为啥要这样做呢？感觉增加了复杂度啊？

实际上，我们在开之前的版本开发过程中会遇到一些很常见的问题，比如两个人或者更多人在合作开发一个项目，数据库都使用本地的，但是由于数据库配置文件是在版本库中的，也就意味着每次拉代码下来以后都要修改一下配置文件的 host ，甚至账号，这是一个很尴尬的场景。这里出现这个场景的原因就是：应该随环境变化的东西被写死了。

所以出现了使用 .env 配置文件，用以存储一些依赖环境的变量，比如数据库配置，因为它不会被加入到版本库中， 所以还用以配置一些敏感信息：比如正式环境的一些第三方应用账号，token 等。

但是现状却是，很多人在使用中想要把所有的配置（或者自己想要改动的配置）都写到 env 中，比如，数据库字符集、邮件标题。然而这些东西从本质上来讲它就应该是这个应用都统一的就不应该这么玩。

还有一件事顺便提一下，数据表前缀，实际上现在很多人已经不需要用它了，它的存在是很久以前大家多个项目共用一个数据库时才用前缀来区分，现在的应用基本都一个应用一个库，所以它已经是一个非常稀有的需求了，更不用说需要写到 .env 了。

一个简单的判断技巧：这项配置是不是需要每个环境都不一样？如果是就放到 env, 否则就写到 config 目录下就好了。

.env 文件永远不会放到版本库，机器上留下一个就好了，要改到机器上去改 .env 就好（这属于部署环节了）。

发布肯定需要另行拷贝.env文件。或者人肉，或者依赖发布（持续集成）工具。

## 数据迁移

迁移（Migration）直接看来是用于定义数据库表结构的。

它是对于数据库的一种「版本控制」。

在调试环境（Homestead）中需要频繁的数据库结构操作，而由于关系数据库存储结构的特点，经常会有「牵一发而动全身」的情况，所以对数据库进行版本控制。

而且，使用 Laravel 内置的迁移不需要你写冗长的 SQL 表结构管理语句，降低了后端开发对数据库操作的门槛。这些操作都可以在迁移中运行而不必写 SQL。

# 安装laravel

```bash
composer create-project --prefer-dist laravel/laravel blog
```

[命令解释](https://docs.phpcomposer.com/03-cli.html#create-project)

## 踩到的坑

### 数据迁移报错

```bash
➜  laravel_test php artisan migrate
Migration table created successfully.

In Connection.php line 647:

  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: alter table `users` add unique `users_email_unique`(`email`))


In Connection.php line 449:

  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes


➜  laravel_test
```

**方法一**

`config/database.php` 文件, 修改数据库的字符集和排序.

关键字搜索： `utf8mb4` 替换成 `utf8` , `utf8mb4_unicode_ci` 替换成 `utf8_general_ci`.

**方法二**

修改 `app/Providers/AppServiceProvider.php` 文件

```php
use Illuminate\Support\Facades\Schema;

public function boot()
{
    Schema::defaultStringLength(191);
}
```





