# 自动加载

本文中的关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY" 和 "OPTIONAL" 在 [RFC 2119](http://tools.ietf.org/html/rfc2119) 中描述解释。

- MUST = REQUIRED = SHALL = 必须

- MUST NOT = SHALL NOT = 绝不

- SHOULD = RECOMMENDED = 应该 = 建议

- SHOULD NOT = NOT RECOMMENDED = 不应该 = 不建议

- MAY = OPTIONAL = 可选 = 可能 = 可以

## 1. 概述

本 PSR 规范描述了从文件路径 [自动加载][] 类。并且本规范完全兼容其他自动加载规范，其中包括 [PSR-0][]。本规范也描述了文件放置到哪里，才会根据规范自动加载。


## 2. 规范

1. 术语 "class" 指的是类, 接口, traits, 以及其他类似的结构。

2. 完全限定的类名具有以下形式:

        \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

    1. 完全限定类名 **必须** 有一个顶级命名空间，这也被称为 "vender 命名空间"。

    2. 完全限定类名 **可以** 有一个或多个子命名空间。

    3. 完全限定类名 **必须** 有一个类名。

    4. 含有下划线的完全限定类名没有任何特别的意义。

    5. 完全限定类名 **可以** 是任意大小写字母字符的组合。

    6. 所有类名 **必须** 是大小写敏感的方式引用。

3. 当加载一个完全限定的类名对应的文件时 ...

    1. 在完全限定的类名中一个或多个前缀的命名空间和子命名空间，每个至少对应一个基本目录，但不
       包含命名空间分隔符。

    2. 命名空间前缀后的子命名空间名称对应的基本目录，子命名空间与对应的子基本目录必须大小写一
       致。其中命名空间的分隔符也是文档目录分隔符。

    3. 类名对应 `.php` 结尾的文件名，文件名 **必须** 与类名相同。

4. 自动加载 **绝不** 要抛出异常，**绝不** 要引发任何级别的错误，并且 **不应该** 返回一个值。


## 3. 示例

下表展示了一个给定的完全限定类名，命名空间前缀和基本目录中的相应文件路径。

| 完全限定类名    | 命名空间前缀   | 基本目录           | 文件路径的结果
| ----------------------------- |--------------------|--------------------------|-------------------------------------------
| \Acme\Log\Writer\File_Writer  | Acme\Log\Writer    | ./acme-log-writer/lib/   | ./acme-log-writer/lib/File_Writer.php
| \Aura\Web\Response\Status     | Aura\Web           | /path/to/aura-web/src/   | /path/to/aura-web/src/Response/Status.php
| \Symfony\Core\Request         | Symfony\Core       | ./vendor/Symfony/Core/   | ./vendor/Symfony/Core/Request.php
| \Zend\Acl                     | Zend               | /usr/includes/Zend/      | /usr/includes/Zend/Acl.php

符合本规范的自动加载实现示例，请参阅 [示例文件][]。该示例绝不要视为本规范的一部分它可能在任何
时间被修改。

[自动加载]: http://php.net/autoload
[PSR-0]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-0.zh.md
[示例文件]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-4-autoloader-examples.zh.md
