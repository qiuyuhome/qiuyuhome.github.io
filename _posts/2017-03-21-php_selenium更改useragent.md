---
layout: post
title: php-selenium更改useragent, 更改浏览器名称
categories: PHP
description: php-selenium更改useragent, 更改浏览器名称
keywords: php, selenium, phantomjs
---

只用phantomjs无头浏览器, 需要更改一下useragent, 在google的文档上一直没找到. 最后在http://www.kancloud.cn/wangking/selenium/235055, 在这里找到了答案. 代码如下:

```php
<?php
namespace Facebook\WebDriver;
use Facebook\WebDriver\Remote\DesiredCapabilities;
use Facebook\WebDriver\Remote\RemoteWebDriver;
use Facebook\WebDriver\Chrome\ChromeOptions;
require_once('vendor/autoload.php');

header("Content-Type: text/html; charset=UTF-8");

// start Firefox with 5 second timeout
$host = 'http://localhost:5555/wd/hub'; // this is the default
$capabilities = DesiredCapabilities::phantomjs();
$useragent = 'Mozilla/5.0 (Linux; U; Android 2.3.7; en-us; Nexus One Build/FRF91) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1';
$capabilities->setCapability("phantomjs.page.settings.userAgent", $useragent);

$driver = RemoteWebDriver::create($host, $capabilities, 55000);
$driver->manage()->timeouts()->implicitlyWait(30);    //隐性设置15秒
$driver->get('http://www.atool.org/useragent.php');
//$driver->get('http://www.ip138.com/useragent/');
echo $driver->findElement(WebDriverBy::id('ua_code'))->getAttribute('value');
echo 'done!';

//关闭浏览器
$driver->quit();
?>
```

最后的输出是:

```
Mozilla/5.0 (Linux; U; Android 2.3.7; en-us; Nexus One Build/FRF91) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1done!
```

证明Ok. 


