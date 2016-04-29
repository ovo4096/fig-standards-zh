自动加载标准
===========

> **已弃用** - 于 2014-10-21 PSR-0 被标记为弃用方案， [PSR-4] 现在作为替代的建议方案。

[PSR-4]: http://www.php-fig.org/psr/psr-4/

下文描述了自动加载机制必须遵守的强制性要求。

强制性要求
---------

* 一个完全限定的命名空间和类必须具有以下结构
 `\<Vendor Name>\(<Namespace>\)*<Class Name>`
* 命名空间必须有一个顶级的名命名空间 `("Vendor Name")`
* 命名空间可以有很多任意的子命名空间
* 当从文件系统加载时，命名空间的分隔符是转换后的 `文档分隔符`
* 类名中 `_` 字符是转换后的 `文档分隔符`。 `_` 字符在命名空间中没有任何特殊意义
* 当从文件系统加载时，完全限定名的命名空间与类的后缀是 `.php`
* vendor 的名称、命名空间、和类名称可以是任意的大写和小写字母字符的组合

示例
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

强调命名空间和类名
-----------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

The standards we set here should be the lowest common denominator for
painless autoloader interoperability. You can test that you are
following these standards by utilizing this sample SplClassLoader
implementation which is able to load PHP 5.3 classes.

实现示例
----------------------

下面一个示例简单地演示上文提出的标准是怎么自动加载的。

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
spl_autoload_register('autoload');
```

SplClassLoader 实现
-----------------------------

下面的 gist 是 SplClassLoader 的实现，如果你按照上面提出的自动加载互操作标准，它可以加载你的类。这些标准目前是 PHP 5.3 类加载的推荐方式。 

* [http://gist.github.com/221634](http://gist.github.com/221634)
