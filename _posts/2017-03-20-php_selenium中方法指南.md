

参考: http://facebook.github.io/php-webdriver/latest/Facebook/WebDriver/Remote/RemoteWebDriver.html

这里有一个示例. 

```php
<?php

// 引入php-webdriver的类. 使用composer自动安装的类. 所以需要自动引入. 
namespace Facebook\WebDriver;
use Facebook\WebDriver\Remote\DesiredCapabilities;
use Facebook\WebDriver\Remote\RemoteWebDriver;
require_once('vendor/autoload.php');


header("Content-Type: text/html; charset=UTF-8");

// start Firefox with 5 second timeout
$waitSeconds = 15;  //需等待加载的时间，一般加载时间在0-15秒，如果超过15秒，报错。

// 运行selenium-server后, 就会开启4444端口. 用来监听
$host = 'http://localhost:4444/wd/hub'; // this is the default


// 返回制定浏览器的资源.
$capabilities = DesiredCapabilities::phantomjs();

// 创建驱动.
$driver = RemoteWebDriver::create($host, $capabilities, 5000);

// 使用创建的驱动来打开指定的URL地址.
$driver->get('https://www.baidu.com/');

// 输出标题
echo '标题1：' . $driver->getTitle() . "\n";


$driver->findElement(WebDriverBy::id('kw'))->sendKeys('wwe')->submit();

// 等待新的页面加载完成....
$driver->wait($waitSeconds)->until(
    WebDriverExpectedCondition::visibilityOfElementLocated(
        WebDriverBy::partialLinkText('100shuai')
    )
);
$driver->findElement(WebDriverBy::partialLinkText('100shuai'))->sendKeys('xxx')->click();    //一般点击链接的时候，担心因为失去焦点而抛异常，则可以先调用一下sendKeys，再click


switchToEndWindow($driver); //切换至最后一个window

// 等待加载....
$driver->wait($waitSeconds)->until(
    WebDriverExpectedCondition::visibilityOfElementLocated(
        WebDriverBy::partialLinkText('SmackDown收视率创历史新低')
    )
);
echo '标题1：' ."：" . $driver->getTitle() . "\n";    //cmd.exe中文乱码，所以需转码

$driver->findElement(WebDriverBy::partialLinkText('SmackDown收视率创历史新低'))->click();

switchToEndWindow($driver); //切换至最后一个window


// 等待加载....
$driver->wait($waitSeconds)->until(
    WebDriverExpectedCondition::titleContains('SmackDown收视率创历史新低')
);
echo '标题1：' ."：" . $driver->getTitle() . "\n";    //cmd.exe中文乱码，所以需转码


//关闭浏览器
$driver->quit();

//切换至最后一个window
//因为有些网站的链接点击过去带有target="_blank"属性，就新开了一个TAB，而selenium还是定位到老的TAB上，如果要实时定位到新的TAB，则需要调用此方法，切换到最后一个window
function switchToEndWindow($driver){

    $arr = $driver->getWindowHandles();
    foreach ($arr as $k=>$v){
        if($k == (count($arr)-1)){
            $driver->switchTo()->window($v);
        }
    }
}

```



