---
layout: post
title: phpcs的使用
categories: PHP
description: phpcs的使用
keywords: PHP, Linux, phpcs
---

遇到队友不按套路出牌, 怎么办, 拟定好代码风格, 开发的时候却不执行. 没关系. 可以使用 `phpcs` 来约束他.
如果还是不行. 只能出绝招了. 在 git 提交前做一个钩子. 代码不符合规则, 则不允许提交. 哈哈. 当然. 我没这么干.
旧的代码还得慢慢改. 哎.

## phpcs安装方法

安装方法. 可以看我的另一个文章.

[composer的global使用](http://www.qiuyuhome.com/2018/01/21/composer%E7%9A%84global%E4%BD%BF%E7%94%A8/)

## phpcs使用

[github 项目地址](https://github.com/squizlabs/PHP_CodeSniffer)

使用也比较简单. 帮助文件也很短.

```
➜  coupons_deals git:(master) ✗ phpcs -h

Usage: phpcs [-nwlsaepqvi] [-d key[=value]] [--colors] [--no-colors]
  [--cache[=<cacheFile>]] [--no-cache] [--tab-width=<tabWidth>]
  [--report=<report>] [--report-file=<reportFile>] [--report-<report>=<reportFile>]
  [--report-width=<reportWidth>] [--basepath=<basepath>] [--bootstrap=<bootstrap>]
  [--severity=<severity>] [--error-severity=<severity>] [--warning-severity=<severity>]
  [--runtime-set key value] [--config-set key value] [--config-delete key] [--config-show]
  [--standard=<standard>] [--sniffs=<sniffs>] [--exclude=<sniffs>]
  [--encoding=<encoding>] [--parallel=<processes>] [--generator=<generator>]
  [--extensions=<extensions>] [--ignore=<patterns>] [--ignore-annotations]
  [--stdin-path=<stdinPath>] [--file-list=<fileList>] <file> - ...

 -     Check STDIN instead of local files and directories
 -n    Do not print warnings (shortcut for --warning-severity=0)
 -w    Print both warnings and errors (this is the default)
 -l    Local directory only, no recursion
 -s    Show sniff codes in all reports
 -a    Run interactively
 -e    Explain a standard by showing the sniffs it includes
 -p    Show progress of the run
 -q    Quiet mode; disables progress and verbose output
 -m    Stop error messages from being recorded
       (saves a lot of memory, but stops many reports from being used)
 -v    Print processed files
 -vv   Print ruleset and token output
 -vvv  Print sniff processing information
 -i    Show a list of installed coding standards
 -d    Set the [key] php.ini value to [value] or [true] if value is omitted

 --help                Print this help message
 --version             Print version information
 --colors              Use colors in output
 --no-colors           Do not use colors in output (this is the default)
 --cache               Cache results between runs
 --no-cache            Do not cache results between runs (this is the default)
 --ignore-annotations  Ignore all @codingStandard annotations in code comments

 <cacheFile>    Use a specific file for caching (uses a temporary file by default)
 <basepath>     A path to strip from the front of file paths inside reports
 <bootstrap>    A comma separated list of files to run before processing begins
 <file>         One or more files and/or directories to check
 <fileList>     A file containing a list of files and/or directories to check (one per line)
 <encoding>     The encoding of the files being checked (default is utf-8)
 <extensions>   A comma separated list of file extensions to check
                The type of the file can be specified using: ext/type
                e.g., module/php,es/js
 <generator>    Uses either the "HTML", "Markdown" or "Text" generator
                (forces documentation generation instead of checking)
 <patterns>     A comma separated list of patterns to ignore files and directories
 <processes>    How many files should be checked simultaneously (default is 1)
 <report>       Print either the "full", "xml", "checkstyle", "csv"
                "json", "junit", "emacs", "source", "summary", "diff"
                "svnblame", "gitblame", "hgblame" or "notifysend" report
                (the "full" report is printed by default)
 <reportFile>   Write the report to the specified file path
 <reportWidth>  How many columns wide screen reports should be printed
                or set to "auto" to use current screen width, where supported
 <severity>     The minimum severity required to display an error or warning
 <sniffs>       A comma separated list of sniff codes to include or exclude from checking
                (all sniffs must be part of the specified standard)
 <standard>     The name or path of the coding standard to use
 <stdinPath>    If processing STDIN, the file path that STDIN will be processed as
 <tabWidth>     The number of spaces each tab represents

➜  coupons_deals git:(master) ✗

```

列出几个比较常用的命令:

**列出已经安装的规范**

```
➜  coupons_deals git:(master) ✗ phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, Squiz and Zend
➜  coupons_deals git:(master) ✗
```

**使用指定的规范**

```
➜  coupons_deals git:(master) ✗ phpcs --standard=PSR2 couponsDeals.class.php
➜  coupons_deals git:(master) ✗
```

## 修改单行字符不能超过120的限制

`psr-2` 规范虽然说不能超过120个字符, 但是也说了, 这个不是硬性限制. 所以, 超过120也不算错.

[psr-2 规范](https://laravel-china.org/topics/2079/psr-specification-psr-2-coding-style-specification)

> 每行的字符数 应该 软性保持在 80 个之内，理论上 一定不可 多于 120 个，但 一定不可 有硬性限制。

怎么改呢? 这个得需要去修改配置了.

一张图说明一切.

![phpcs_set_2](/images/posts/phpcs_set_2.png)