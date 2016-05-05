编程风格指南
==================

本指南延伸并且扩展了 [PSR-1] 基础编程编程标准。

本指南通过列举一系列共享的规则集和关于怎么去格式化为期望的 PHP 代码，以减少阅读不同作者代码时
的理解难度。

这里的风格规则是不同成员项目的共同之处。不同的作者在多个项目中进行合作时，使用一套准则有助于这
些项目。因此，本指南的好处不是规则本身，而是共享使用这些规则。

本文中的关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY" 和 "OPTIONAL" 在 [RFC 2119] 中描述解释。

- MUST = REQUIRED = SHALL = 必须

- MUST NOT = SHALL NOT = 绝不

- SHOULD = RECOMMENDED = 应该 = 建议

- SHOULD NOT = NOT RECOMMENDED = 不应该 = 不建议

- MAY = OPTIONAL = 可选 = 可能

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-0.zh.md
[PSR-1]: https://github.com/owwo/fig-standards-zh/blob/master/accepted/PSR-1-basic-coding-standard.zh.md


1. 概要
-----------

- 代码 **必须** 遵循 "编程风格指南" PSR [[PSR-1]] 的标准。

- 代码 **必须** 使用 4 空格作为缩进，不是制表符 (tab) 。

- **绝不** 要硬限制行的长度; 软限制 **必须** 是 120 个字符; 行长度 **应该** 保持在 80 字
  符或更少。

- `namespace` 声明后 **必须** 要有一行空行， `use` 声明块区后也要有一行空行。

- 类的开括号 `{` **必须** 在声明的下一行，而闭括号 `}` **必须** 在类体的下一行。

- 方法的开括号 `{` **必须** 在声明的下一行，而闭括号 `}` **必须** 在方法体的下一行。

- 所有属性和方法的访问修饰符 **必须** 显式声明; `abstract` 和 `final` **必须** 在访问修
  饰符之前声明; `static`  **必须** 在访问修饰符之后声明。

- 控制结构关键字之后 **必须** 有一个空格，而方法和函数调用 **绝不** 要这样做。

- 控制结构的开括号 `{` **必须** 在同行，而闭括号 `}` **必须** 在控制结构体之后的下一行。

- 控制结构的开括号 `(` 之后 **绝不** 要有一个空格，闭括号 `)` 之前也 **绝不** 要有一个空格
  。

### 1.1. 示例

本示例做为一个快速概览，涵盖了下面的一些规则:

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // 方法体
    }
}
```

2. 常规
----------

### 2.1 基础编程标准

代码 **必须** 遵循 [PSR-1] 列出的全部规则。

### 2.2 文件

所有 PHP 文件 **必须** 使用 Unix LF (换行符) 作为行结尾。

所有 PHP 文件 **必须** 以一个单独的空行作为结尾。

闭标签 `?>` **必须** 从只包含 PHP 的文件中省略。

### 2.3. 行

**绝不** 要硬性限制行的长度。

软性限制行长度 **必须** 是120个字符，软性限制在使用自动风格检查时 **必须** 使用警告提示，但
**绝不** 要使用错误提示。

行长度 **不应该** 大于 80 个字符，行长度大于 80 个字符 **应该** 被分割为每行不超过 80 字符
的多个连续的行。

非空行 **绝不** 要有尾空格。

空行 **可能** 为了区分代码块以提高代码可读性被添加。

每行语句 **绝不** 要多于一条。

### 2.4. 缩进

代码必须使用 4 个空格作为缩进，并且绝不要使用制表符 (tab) 作为缩进。

> 备注: 只使用空格，而不是空格混合制表符 (tab)，有助于避免比较文件差异, 补丁, 历史, 注释的问
> 题。使用空格也可以很容易的精细划分插入行的子缩进进行对齐。

### 2.5. 关键字与 True/False/Null

PHP 中的关键字 ([keywords]) **必须** 使用小写字母。

PHP 中的常量 `true`, `false`, 与 `null` **必须** 使用小写字母。

[keywords]: http://php.net/manual/en/reserved.keywords.php



3. Namespace 与 Use 的声明
---------------------------------

如果有 `namespace` 的声明，在这之后 **必须** 添加一行空行。

如果有 `use` 的声明，所有 `use` 声明 **必须** 添加在 `namespace` 声明之后。

每条 `use` 声明 **必须** 只使用一个 `use` 关键字。

`use` 声明块之后 **必须** 有一行空行。

示例:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... 其他的 PHP 代码 ...

```


4. 类, 属性, 和方法
-----------------------------------

术语 "class" 指代全部的类, 接口, 和 traits。

### 4.1. Extends 与 Implements

`extends` 和 `implements` 关键字必须被声明在类 `class` 名的同一行。

类 `class` 的开括号 `{` **必须** 单独占用一行，闭括号 `}` **必须** 在类体的下一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // 常量, 属性, 方法
}
```

`implements` 列表可能被分割为多个不同的行，每个分割行需要缩进一次。当这样做，列表的第一条
**必须** 被放在下一行，并且每个接口 **必须** 占用一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // 常量, 属性, 方法
}
```

### 4.2. 属性

访问修饰符 **必须** 声明在所有属性上。

`var` 关键字 **绝不** 要使用在属性声明当中。

**绝不** 要在一条语句声明多个属性。

属性名 **不应该** 前缀一个下划线去指明是 protected 或者是 private 的可见性。

属性声明看起来像下面这样:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. 方法

访问修饰符 **必须** 声明在所有方法上。

方法名 **不应该** 前缀一个下划线去指明是 protected 或者是 private 的可见性。

方法声明中的方法名之后 **绝不** 要有一个空格，开括号 `{` **必须** 单独占用一行，闭括号 `}`
**必须** 在方法体的下一行， **绝不** 要在开括号 `(` 之后添加一个空格，也 **绝不** 要在闭括
号 `)` 之前添加一个空格。

方法声明看起来像下面这样，注意括号 `()`, 逗号, 空格, 和大括号 `{}` 的放置位置:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // 方法体
    }
}
```    

### 4.4. 方法的参数

在参数列表中，每个逗号前 **绝不** 要添加一个空格，逗号之后 **必须** 添加一个空格。

方法参数有默认值时 **必须** 添加在参数列表的结尾。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // 方法体
    }
}
```

参数列表 **可能** 被分割为多行，每个子分割行需要缩进一次。当这样做时，第一个参数 **必须** 被
放在下一行，并且其余的每个参数 **必须** 占用一行。

当参数列表被分割为多行时，闭括号 `)` 与开括号 `{` **必须** 将它们放置在单独的一行，并且使用
一个空格在他们之间隔开。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // 方法体
    }
}
```

### 4.5. `abstract`, `final`, 和 `static`

若有 `abstract` 和 `final` 声明 **必须** 优先于访问修饰符。

若有 `static` 声明 **必须** 放置在访问修饰符之后。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // 方法体
    }
}
```

### 4.6. 方法与函数的调用

当调用一个方法或者一个函数时，**绝不** 要在方法或函数名与开括号 `(` 之间添加一个空格，
**绝不** 要在开括号 `(` 之后添加一个空格，也 **绝不** 要在闭括号 `)` 之前添加一个空格。在参
数列表中，**绝不** 要在每个逗号之前添加一个空格，而每个逗号之后 **必须** 添加一个空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

参数列表 **可能** 被分割为多行，这样做时，每个子行需要缩进一次，每个参数 **必须** 单独占用一
行。

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. 控制结构
---------------------

控制解构的风格规则一般如下:

- 控制结构关键字之后 **必须** 添加一个空格
- 开括号 `(` 之后 **绝不** 要添加一个空格
- 闭括号 `)` 之前 **绝不** 要添加一个空格
- 闭括号 `)` 和开括号 `{` 之间 **必须** 添加一个空格
- 结构体 **必须** 被缩进一次
- 闭括号 `}` **必须** 放在结构体的下一行

每个结构体 **必须** 用大括号 `{}` 括起来。这样有助于降低添加新行时发生错误的可能性。


### 5.1. `if`, `elseif`, `else`

一个 `if` 结构看起来像下面这样。注意其中括号 `()` 与大括号 `{}` 的放置位置， `else` 和
`elseif` 与之前结构体的闭括号 `}` 在同一行。

```php
<?php
if ($expr1) {
    // if 体
} elseif ($expr2) {
    // elseif 体
} else {
    // else 体;
}
```

`elseif` 关键字 **应该** 替代 `else if` 这样所有的控制关键字看起来都是一个单词。

### 5.2. `switch`, `case`

一个 `switch` 结构看起来像下面这样，注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置，
`switch` 中的 `case` 语句 **必须** 被缩进一次，`break` 关键字 (或其它终止关键字)
**必须** 在同级的 `case` 体中缩进。当需要贯通执行时 **必须** 在一个不为空的 `case` 体中添
加一条注释诸如 `// no break`。

```php
<?php
switch ($expr) {
    case 0:
        echo '第一条 case 语句，与 break';
        break;
    case 1:
        echo '第二条 case 语句，与贯通执行';
        // no break
    case 2:
    case 3:
    case 4:
        echo '第三条 case 语句, return 替代 break';
        return;
    default:
        echo '缺省值 case';
        break;
}
```


### 5.3. `while`, `do while`

一个 `while` 语句看起来像下面这样。注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置。

```php
<?php
while ($expr) {
    // 结构体
}
```

同样一个 `do while` 看起来像下面这样。注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置。

```php
<?php
do {
    // 结构体;
} while ($expr);
```

### 5.4. `for`

一个 `for` 语句看起来像下面这样。注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置。

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for 体
}
```

### 5.5. `foreach`

一个 `foreach` 语句看起来像下面这样。注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置。

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach 体
}
```

### 5.6. `try`, `catch`

一个 `try catch` 块看起来像下面这样。注意其中括号 `()`, 空格, 以及大括号 `{}` 的放置位置。

```php
<?php
try {
    // try 体
} catch (FirstExceptionType $e) {
    // catch 体
} catch (OtherExceptionType $e) {
    // catch 体
}
```

6. 闭包 (Closures)
-----------

闭包声明 **必须** 在 `function` 关键字的空格之后，并且 `use` 关键字的前后要有一个空格。

开括号 `{` **必须** 在同一行，闭括号 `}` **必须** 放在闭包体的下一行。

**绝不** 要在参数列表或变量列表的开括号 `(` 之后添加一个空格，也 **绝不** 要添加一个空格在闭
括号 `)` 在之前。

参数列表和变量列表中，**绝不** 要在每个逗号之前添加空格，但是 **必须** 在每个逗号之后添加一个空格。

闭包参数含有的默认值的参数 **必须** 放在参数列表结尾处。

一个闭包声明看起来像下面这样。注意其中括号 `()`, 逗号, 空格, 以及大括号 `{}` 的放置位置:

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // 体
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // 体
};
```

参数列表和变量列表 **可能** 被分割为多行，当这样做时，每个子行需要缩进一次，每个参数或变量
**必须** 独立占用一行。

When the ending list (whether or arguments or variables) is split across
multiple lines, the closing parenthesis and opening brace MUST be placed
together on their own line with one space between them.

The following are examples of closures with and without argument lists and
variable lists split across multiple lines.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

Note that the formatting rules also apply when the closure is used directly
in a function or method call as an argument.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


7. Conclusion
--------------

There are many elements of style and practice intentionally omitted by this
guide. These include but are not limited to:

- Declaration of global variables and global constants

- Declaration of functions

- Operators and assignment

- Inter-line alignment

- Comments and documentation blocks

- Class name prefixes and suffixes

- Best practices

Future recommendations MAY revise and extend this guide to address those or
other elements of style and practice.


Appendix A. Survey
------------------

In writing this style guide, the group took a survey of member projects to
determine common practices.  The survey is retained herein for posterity.

### A.1. Survey Data

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. Survey Legend

`indent_type`:
The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"

`line_length_limit_soft`:
The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`line_length_limit_hard`:
The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`class_names`:
How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.

`class_brace_line`:
Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?

`constant_names`:
How are class constants named? `upper` = Uppercase with underscore separators.

`true_false_null`:
Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?

`method_names`:
How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.

`method_brace_line`:
Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?

`control_brace_line`:
Does the opening brace for a control structure go on the `same` line, or on the `next` line?

`control_space_after`:
Is there a space after the control structure keyword?

`always_use_control_braces`:
Do control structures always use braces?

`else_elseif_line`:
When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?

`case_break_indent_from_switch`:
How many times are `case` and `break` indented from an opening `switch` statement?

`function_space_after`:
Do function calls have a space after the function name and before the opening parenthesis?

`closing_php_tag_required`:
In files containing only PHP, is the closing `?>` tag required?

`line_endings`:
What type of line ending is used?

`static_or_visibility_first`:
When declaring a method, does `static` come first, or does the visibility come first?

`control_space_parens`:
In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
Is there a blank line after the opening PHP tag?

`class_method_control_brace`:
A summary of what line the opening braces go on for classes, methods, and control structures.

### A.3. Survey Results

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
