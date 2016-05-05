基础编程标准
=====================

标准的这部分包括那些编程元素应该被认为是必要的，以确保共享的 PHP 代码在较高技术层面的互用性。

本文中的关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY" 和 "OPTIONAL" 在 [RFC 2119] 中描述解释。

- MUST = REQUIRED = SHALL = 必须

- MUST NOT = SHALL NOT = 绝不

- SHOULD = RECOMMENDED = 应该 = 建议

- SHOULD NOT = NOT RECOMMENDED = 不应该 = 不建议

- MAY = OPTIONAL = 可选

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-0.zh.md
[PSR-4]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-4-autoloader.zh.md


1. 概要
-----------

- 文件 **必须** 只使用 `<?php` 和 `<?=` 标签。

- PHP 代码文件 **必须** 只使用 UTF-8 无 BOM 编程。

- 文件 **应该** *只* 包含声明 ( 类, 函数, 常量...等 ) *或者* 引起副作用的操作 (例如 生成
  输出的, 改变 .ini 设置,  等等) 但 **不应该** 将两者放在一起。

- 命名空间和类 **必须** 遵循 "自动加载" PSR: [[PSR-0], [PSR-4]] 的标准。

- 类名 **必须** 使用 `StudlyCaps` 大写字母开头的 Pascal 命名方式。

- 类常量 **必须** 使用全部大写字母和下划线作为分割符的命名方式。

- 方法名 **必须** 使用 `camelCase` 小写字母开头的小驼峰命名方式。


2. 文件
--------

### 2.1. PHP 标签

PHP 代码 **必须** 使用长标签 `<?php ?>` 或短标签 `<?= ?>`， **绝不** 要使用其他不同的标
签。

### 2.2. 字符编码

PHP 代码 **必须** 只使用 UTF-8 无 BOM 编码。

### 2.3. 副作用

一个文件 **应该** 包含新声明 ( 类, 函数, 常量...等 ) 和不会造成其他副作用的操作，或者它
**应该** 是一段运行后会引起副作用的逻辑操作，但 **不应该** 将两者放在一起。

"副作用" 这个词组描述的执行逻辑不直接关联到类声明, 函数, 常量...等， *仅仅在这个包含的文件中*
。

"副作用" 包括不限于: 生成输出, 明确使用 `require` 或 `include` , 连接到扩展服务, 修改
ini 设置, 发出错误或异常, 修改全局或静态变量, 读取或写入一个文件...等。

下面的例子是文件同时具有声明与副作用，即这个是需要避免的一个例子:

```php
<?php
// 副作用: 修改 ini 设置
ini_set('error_reporting', E_ALL);

// 副作用: 加载一个文件
include "file.php";

// 副作用: 产生一个输出
echo "<html>\n";

// 声明
function foo()
{
    // 函数体
}
```

下面这个例子是文件包含声明但无副作用，即这个是需要效仿的一个例子:

```php
<?php
// 声明
function foo()
{
    // 函数体
}

// 有条件的声明 *无* 副作用
if (! function_exists('bar')) {
    function bar()
    {
        // 函数体
    }
}
```


3. 命名空间与类名
----------------------------

命名空间和类 **必须** 遵循 "自动加载" PSR: [[PSR-0], [PSR-4]] 的标准。

这意味着每个类是一个文件，并且至少有一级命名空间，即顶级的 vendor 名称。

类名 **必须** 使用 `StudlyCaps` 大写字母开头的 Pascal 命名方式。

代码写自 PHP 5.3 或更高版本 **必须** 使用 namespace 格式。

示例:

```php
<?php
// PHP 5.3 或更高版本:
namespace Vendor\Model;

class Foo
{
}
```

代码写自 5.2.x 或更早的版本 **应该** 使用伪命名空间 (pseudo-namespacing) 转换，即类名加
`Vendor_` 前缀。

```php
<?php
// PHP 5.2.x 或更早的版本:
class Vendor_Model_Foo
{
}
```

4. 类常量, 属性, 以及方法
-------------------------------------------

术语 "类 (class)" 指的是所有类, 接口, traits。

### 4.1. 常量

类常量 **必须** 使用全部大写字母和下划线作为分割符的命名方式。

示例:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. 属性

本指南有意避免任何关于建议使用 `$StudlyCaps`, `$camelCase`, 或 `$under_score` 的命名方
式在属性命名中。

无论命名规范如何都 **应该** 在一个合理的范围内实行一贯性原则。这个范围可能是 vendor-level,
package-level, class-level, 或 method-level。

### 4.3. 方法

方法名 **必须** 使用 `camelCase()` 小写字母开头的小驼峰命名方式。
