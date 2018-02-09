---
layout: post
title: ThinkPHP学习笔记, 执行流程
categories: PHP
description: ThinkPHP学习笔记, 执行流程
keywords: PHP, ThinkPHP
---

# ThinkPHP学习笔记, 执行流程, 不断更新ing


## 入口文件
1. 检测PHP环境
2. 设置调试模式
3. 定义应用目录
4. 引入ThinkPHP入口文件

## ThinkPHP公共入口文件
1. 记录开始运行时间，记录内存初始使用
2. URL 模式定义
3. 类文件后缀
4. 系统常量定义
5. 系统信息

	判断php版本，进行`MAGIC_QUOTES_GPC`，`get_magic_quotes_gpc`设置, 是为了解决数据遇到 单引号' 和 双引号" 以及 反斜线\ 时自动加上反斜线，进行自动转义.
6. 判断运行模式

	根据当前的运行模式, 设置常量`_PHP_FILE_`和`_ROOT_`
	
8. 加载核心Think类: ThinkPHP 引导类

	`require CORE_PATH.'Think'.EXT;`
	 `/thinkphp_3.2.3_full/ThinkPHP/Library/Think/Think.class.php`
9. 应用初始化 

	执行第8步中的start()

## ThinkPHP 引导类
#### 1. 注册AUTOLOAD方法

#### 2. 设定错误和异常处理

#### 4. 初始化文件存储方式
`Storage::connect(STORAGE_TYPE);`

***注:***
在`ThinkPHP 引导类`的第1步中, 已经注册了AUTOLOAD方法, 那么, 运行这个代码的时候, 就会自动加载`/ThinkPHP/Library/Think/Storage.class.php`中的connect方法. 这个方法实例化了`/ThinkPHP/Library/Think/Storage/Driver/File.class.php`, 另外, 这个类中, 如果执行了没有的方法. 还会执行`__callstatic`方法, 然后还是实例化`/ThinkPHP/Library/Think/Storage/Driver/File.class.php`.

实例化类, 本地文件写入存储类, Think\Storage\Driver\File.php
主要功能就是:
1. 文件内容读取
2. 文件写入
3. 文件追加写入
4. 加载文件
5. 文件是否存在
6. 文件删除
7. 读取文件信息

```
    static public function connect($type='File',$options=array()) {
        $class  =   'Think\\Storage\\Driver\\'.ucwords($type);
        self::$handler = new $class($options);
    }
```

获取编译缓存文件, `./Application/Runtime/common~runtime.php`. 如果非DEBUG模式且存在, 就加载.  


#### 5. 读取配置文件. ThinkPHP 普通模式定义
`/Users/qiuyu/www/thinkphp_3.2.3_full/ThinkPHP/Mode/common.php`
包括:
1. 配置文件: 系统惯例配置, 应用公共配置 `'config'    =>  array(...);`
2. 别名定义 `'alias'     =>  array(...);`
3. 函数和类文件 `'core'=array(...);`
4. 行为扩展定义 `'tags'  =>  array(...);`

#### 6. 加载核心文件
```
    // 函数和类文件
    'core'      =>  array(
    	// 框架函数文件, 位置: /ThinkPHP/Common/functions.php
        THINK_PATH.'Common/functions.php', 
        // 项目函数文件, 位置: ./Application/Common/Common/function.php
        COMMON_PATH.'Common/function.php',
        // 系统钩子实现类, 位置: /ThinkPHP/Library/Think/Hook.class.php
        CORE_PATH . 'Hook'.EXT,
        // 应用程序类 执行应用过程管理, 位置: /ThinkPHP/Library/Think/App.class.php
        CORE_PATH . 'App'.EXT,
        // 调度类,完成URL解析、路由和调度 位置: 完成URL解析、路由和调度
        CORE_PATH . 'Dispatcher'.EXT,
        //CORE_PATH . 'Log'.EXT,
        // 路由解析类, 位置: /ThinkPHP/Library/Think/Route.class.php
        CORE_PATH . 'Route'.EXT,
        // 控制器基类 抽象类, 位置: /ThinkPHP/Library/Think/Controller.class.php
        CORE_PATH . 'Controller'.EXT,
        // 试图类, 位置: /ThinkPHP/Library/Think/View.class.php
        CORE_PATH . 'View'.EXT,
        // 行为类, 创建Lite运行文件, 位置: /ThinkPHP/Library/Behavior/BuildLiteBehavior.class.php
        BEHAVIOR_PATH . 'BuildLiteBehavior'.EXT,
        // 系统行为扩展：模板解析, 位置: /ThinkPHP/Library/Behavior/ParseTemplateBehavior.class.php
        BEHAVIOR_PATH . 'ParseTemplateBehavior'.EXT,
        // 系统行为扩展：模板内容输出替换, 位置: /ThinkPHP/Library/Behavior/ContentReplaceBehavior.class.php
        BEHAVIOR_PATH . 'ContentReplaceBehavior'.EXT,
    ),
```

引入`controller.class.php`, 有一个构造方法. 

```
   /**
     * 架构函数 取得模板对象实例
     * @access public
     */
    public function __construct() {
        Hook::listen('action_begin',$this->config);
        //实例化视图类
        $this->view     = Think::instance('Think\View');
        //控制器初始化
        if(method_exists($this,'_initialize'))
            $this->_initialize();
    }
```

会实例化试图类.
#### 7. 加载应用模式配置文件
```php
   // 配置文件
    'config'    =>  array(
        THINK_PATH.'Conf/convention.php',   // 系统惯例配置
        CONF_PATH.'config'.CONF_EXT,      // 应用公共配置
    ),
```
啊


涉及到的内容: 
`C()`函数中, 定义了一个静态的属性: `$_config = array()`. 
通过`C()`函数. 把这些配置文件. 都合并到`$_config`中. 

这里把这2个配置文件都合并到了`$_config`中.

#### 8. 读取当前应用模式对应的配置文件
```
    // 别名定义
    'alias'     =>  array(
        'Think\Log'               => CORE_PATH . 'Log'.EXT,
        'Think\Log\Driver\File'   => CORE_PATH . 'Log/Driver/File'.EXT,
        'Think\Exception'         => CORE_PATH . 'Exception'.EXT,
        'Think\Model'             => CORE_PATH . 'Model'.EXT,
        'Think\Db'                => CORE_PATH . 'Db'.EXT,
        'Think\Template'          => CORE_PATH . 'Template'.EXT,
        'Think\Cache'             => CORE_PATH . 'Cache'.EXT,
        'Think\Cache\Driver\File' => CORE_PATH . 'Cache/Driver/File'.EXT,
        'Think\Storage'           => CORE_PATH . 'Storage'.EXT,
    ),
```

就是把这些赋值到私有属性$_map中
#### 9. 加载模式别名定义
`./Application/Common/Conf/alias.php`
就是加载项目中自定义的别名文件

#### 10. 加载模式行为定义
```
    // 行为扩展定义
    'tags'  =>  array(
        'app_init'     =>  array(
            'Behavior\BuildLiteBehavior', // 生成运行Lite文件
        ),        
        'app_begin'     =>  array(
            'Behavior\ReadHtmlCacheBehavior', // 读取静态缓存
        ),
        'app_end'       =>  array(
            'Behavior\ShowPageTraceBehavior', // 页面Trace显示
        ),
        'view_parse'    =>  array(
            'Behavior\ParseTemplateBehavior', // 模板解析 支持PHP、内置模板引擎和第三方模板引擎
        ),
        'template_filter'=> array(
            'Behavior\ContentReplaceBehavior', // 模板输出替换
        ),
        'view_filter'   =>  array(
            'Behavior\WriteHtmlCacheBehavior', // 写入静态缓存
        ),
    ),
```

Hook类中有个私有的静态属性$tags. 就是把上面的配置赋值给它.

#### 11. 加载应用行为定义
`./Application/Common/Conf/tags.php`
同上, 只是这个是加载项目中自定义的行为配置危险.

#### 12. 加载框架底层语言包
惯例配置文件中有语言的配置. 
`'DEFAULT_LANG'          =>  'zh-cn', // 默认语言`
然后加载这个语言包
` L(include THINK_PATH.'Lang/'.strtolower(C('DEFAULT_LANG')).'.php');`

#### 13. 调试模式加载系统默认的配置文件
`/ThinkPHP/Conf/debug.php`

```
/**
 * ThinkPHP 默认的调试模式配置文件
 */
defined('THINK_PATH') or exit();
// 调试模式下面默认设置 可以在应用配置目录下重新定义 debug.php 覆盖
return  array(
    'LOG_RECORD'            =>  true,  // 进行日志记录
    'LOG_EXCEPTION_RECORD'  =>  true,    // 是否记录异常信息日志
    'LOG_LEVEL'             =>  'EMERG,ALERT,CRIT,ERR,WARN,NOTIC,INFO,DEBUG,SQL',  // 允许记录的日志级别
    'DB_FIELDS_CACHE'       =>  false, // 字段缓存信息
    'DB_DEBUG'				=>  true, // 开启调试模式 记录SQL日志
    'TMPL_CACHE_ON'         =>  false,        // 是否开启模板编译缓存,设为false则每次都会重新编译
    'TMPL_STRIP_SPACE'      =>  false,       // 是否去除模板文件里面的html空格与换行
    'SHOW_ERROR_MSG'        =>  true,    // 显示错误信息
    'URL_CASE_INSENSITIVE'  =>  false,  // URL区分大小写
);
```

#### 14. 读取应用调试配置文件
`./Application/Common/Conf/debug.php`

#### 15. 读取当前应用状态对应的配置文件

#### 16. 设置系统时区

#### 17. 检查应用目录结构 如果不存在则自动创建

#### 18. 记录加载文件时间
` G('loadTime');`

#### 19. 运行应用
`App::run();`

===

## Thinkphp\App  应用程序类 执行应用过程管理
#### 1. 根据上步, 运行`App::run();`
运行应用实例 入口文件使用的快捷方法.

```
   /**
     * 运行应用实例 入口文件使用的快捷方法
     * @access public
     * @return void
     */
    static public function run() {
        // 应用初始化标签
        Hook::listen('app_init');
        App::init();
        // 应用开始标签
        Hook::listen('app_begin');
        // Session初始化
        if(!IS_CLI){
            session(C('SESSION_OPTIONS'));
        }
        // 记录应用初始化时间
        G('initTime');
        App::exec();
        // 应用结束标签
        Hook::listen('app_end');
        return ;
    }
```

#### 2. App::init();
1. 加载动态应用公共文件和配置
2. 日志目录转换为绝对路径 默认情况下存储到公共模块下面
3. 定义当前请求的系统常量
4. URL调度

	` Dispatcher::dispatch();`
	
	文件: `/ThinkPHP/Library/Think/Dispatcher.class.php`
	这个文件就只是在这里用而已. 
	
	
```
	 	$varPath        =   C('VAR_PATHINFO');
        $varAddon       =   C('VAR_ADDON');
        $varModule      =   C('VAR_MODULE');
        $varController  =   C('VAR_CONTROLLER');
        $varAction      =   C('VAR_ACTION');
        $urlCase        =   C('URL_CASE_INSENSITIVE');
```
这些都是惯例配置中的设置. 这里获取到这些配置. 
URL调度, 就是获取当前的controller, action, url, module, 等等都是什么, 然后统统的赋值给常量, 方便以后调用. 

	
开启子域名部署
分析PATHINFO信息
定义PAHTINFO的分隔符
	
```
$depr = C('URL_PATHINFO_DEPR');
define('MODULE_PATHINFO_DEPR',  $depr);
```

#### 3. 全局安全过滤

#### 4. URL调度结束标签
#### 5. 执行应用程序
`App::exec();`
就是根据url, 实例化对应的类. 然后找到方法且执行方法.
重点是: `self::invokeAction($module,$action);`

```
$method =   new \ReflectionMethod($module, $action);
$method->invoke($module);
```

在这里, 执行方法前操作和方法后操作. 

#至此, 流程结束








	






