# laravel Eloquent 解读



```php
AdminUser::where('id', '<', 4)->orderByDesc('id')->get()->toArray();
```

这个到底是如何实现的. 想要知道, 那就得看源码了. 

首先要了解几个概念. 

## ORM

ORM 就是通过实例对象的语法，完成关系型数据库的操作的技术，是"对象-关系映射"（Object/Relational Mapping） 的缩写。

-   数据库的表（table） --> 类（class）
-   记录（record，行数据）--> 对象（object）
-   字段（field）--> 对象的属性（attribute）

>   [ORM 实例教程](http://www.ruanyifeng.com/blog/2019/02/orm-tutorial.html)



在 laravel 中, 也提供了一个 ORM, 命中叫 `Eloquent ORM`, 实现的模型是 `ActiveRecord`.



官方的解释是: Laravel 的 Eloquent ORM 提供了一个漂亮、简洁的 ActiveRecord 实现来和数据库交互。每个数据库表都有一个对应的「模型」用来与该表交互。你可以通过模型查询数据表中的数据，以及在数据表中插入新记录



