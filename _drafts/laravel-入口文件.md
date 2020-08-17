# laravel source code index



入口文件

```php
<?php
    
define('LARAVEL_START', microtime(true));

require __DIR__.'/../vendor/autoload.php';

$app = require_once __DIR__.'/../bootstrap/app.php';

$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);

$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
);

$response->send();

$kernel->terminate($request, $response);

```



这个就是入口文件. 同时, 能够看出laravel总的方案. 



1.  注册核心服务.

```php
$app = require_once __DIR__.'/../bootstrap/app.php';
```

返回了一个laravel封装的容器. 在这个容器中, 注册了 `Illuminate\Contracts\Http\Kernel::class`, `Illuminate\Contracts\Console\Kernel::class`, `Illuminate\Contracts\Debug\ExceptionHandler::class`. 以便后面的使用. 



2.  解析 http 服务.

```php
$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);
```

经过步骤一的注册后, 在这里进行了解析. 用来处理 http 的请求. 



3.  处理请求. 获得响应. 

```php
$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
);
```

4.  返回响应.

```php
$response->send();
```



5.  处理响应后的作业. 

```php
$kernel->terminate($request, $response);
```













