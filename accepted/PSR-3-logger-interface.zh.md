日志接口
================

本文制定了日志库的通用接口。

主要目标是容许类库获得一个 `Psr\Log\LoggerInterface` 对象使写日志简单而通用。框架和 CMS
**可能** 需要扩展接口用于自己的目的，但是 **应该** 与本文档保持兼容。这样可以确保程序使用第三
方类库时可以写日志到集中化的程序日志。

本文中的关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY" 和 "OPTIONAL" 在 [RFC 2119] 中描述解释。

- MUST = REQUIRED = SHALL = 必须

- MUST NOT = SHALL NOT = 绝不

- SHOULD = RECOMMENDED = 应该 = 建议

- SHOULD NOT = NOT RECOMMENDED = 不应该 = 不建议

- MAY = OPTIONAL = 可选 = 可能 = 可以

`实现者` 这个词在本文档被解读为一个实现了 `LoggerInterface` 的日志类库或框架。日志用户被简
称为 `用户`。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. 规范
-----------------

### 1.1 基础

- `LoggerInterface` 公开了 8 个方法去写 8 个等级的日志 (debug, info, notice,
  warning, error, critical, alert, emergency) [RFC 5424] 中有详细解释。

- 第 9 个方法 `log` 接受一个日志等级作为第一个参数。调用这个方法结果 **必须** 与指定日志等级
  的方法结果相同。若调用这个方法时传递的日志等级不是本规范制定的 **必须** 丢出一个
  `Psr\Log\InvalidArgumentException` 异常。用户 **不应该** 使用一个非当前实现支持的自定
  义等级。

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 消息

- 每个方法接受一个字符串作为消息，或一个对象的 `__toString()` 方法。实现者 **可能** 对传递
  的对象进行特殊处理，如果不是这样，实现者 **必须** 转换它到一个字符串。

- 消息 **可以** 包含占用符，实现者 **可以** 结合上下文数组替换该值。

  占用符名称 **必须** 对应上下文数组的键。

  占用符名称 **必须** 被单个开括号 `{` 与 `}` 包含，括号 `{}` 与占用符名称之间 **绝不**
  要有任何空格。

  占用符名称 **应该** 只由字符 `A-Z`, `a-z`, `0-9`, 下划线 `_`, 以及句号 `.` 构成。其他
  字符作为未来占用符规范修改使用的保留字符。

  实现者可以使用占用符转义生成日志。而用户如果不知道上下文将显示什么值，**不应该** 提前转义占
  用符。

  以下占用符作为插值的参考例子:

  ```php
  /**
   * 使用上下文值替换消息占用符
   */
  function interpolate($message, array $context = array())
  {
      // 构建一个由括号 `{}` 包含上下文键的替换数组
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // 替换消息占用符并返回
      return strtr($message, $replace);
  }

  // 含有括号 `{}` 包裹的占用符名称的消息
  $message = "User {username} created";

  // 含有占用符名指向替换值的上下文数组
  $context = array('username' => 'bolivar');

  // 输出 "User bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 上下文

- 每个方法接受一个数组作为上下文的数据，用来持有字符串无关的信息，该数组可以包含任何内容。实现
  者 **必须** 确保对上下文数据尽可能多的宽容。给定上下文一个值 **绝不** 要抛出异常也不要引发
  任何 PHP error, warning 或 notice。

- 如果 `Exception` 传递给上下文数据，它 **必须** 包含在 `'exception'` 键当中。记录异常是
  一个非常普遍的模式，实现者可以在后端需要它时提取堆栈跟踪。实现者在每次使用它时仍要 **必须**
  验证 `'exception'` 键值是一个真正的 `Exception`，因为它 **可能** 包含任何内容。

### 1.4 助手类与接口

- 通过继承 `Psr\Log\AbstractLogger` 并实现通用的 `log` 方法，其他的 8 个方法就会传递消息
  和上下文给它，使你实现 `LoggerInterface` 变得非常容易。

- 同样，若使用 `Psr\Log\LoggerTrait` 只要实现通用的 `log` 方法就可以了。注意 trait 不能
  实现接口，所以你仍要实现 `LoggerInterface`。

- 一起提供的接口 `Psr\Log\NullLogger`，在无日志时，可以通过使用这个接口实现提供一个空记录，
  不过在上下文数据创建代价昂贵的情况，条件记录可能是更好途径。

- `Psr\Log\LoggerAwareInterface` 只包含一个 `setLogger(LoggerInterface $logger)`
  方法框架可以使用它实现自动连接任意的日志记录实例。

- `Psr\Log\LoggerAwareTrait` trait 你可以在任何类中通过它的 `$this->logger` 简单实现
  相同的接口。

- `Psr\Log\LogLevel` 类包含 8 个日志等级的常量。


2. 包
----------

本文描述的接口和类相关的异常类以及一系列验证你实现的测试提供在 [psr/log](https://packagist.org/packages/psr/log) 包中。

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * 日志记录实例
 *
 * 消息必须是字符串或实现 __toString() 的对象
 *
 * 消息可能包含占位符: {foo} 这里 foo 将被替换为上下文数据的键 "foo"
 *
 * 上下文数组可以包含任何数据，只有在作为异常实例进行堆栈跟踪时，它的键名必须是 "exception"
 *
 * https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * 阅读完整的实例规范
 */
interface LoggerInterface
{
    /**
     * 系统不可用
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * 必须立即采取的行动
     *
     * 例如: 网站崩溃, 数据库不可以用, 等等。这应该发送一条短信来提醒你
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * 紧急情况
     *
     * 例如: 程序组件不可用, 非预期的异常
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * 运行时的错误这不需要立即处理，但应该被记录下来
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * 非错误性的异常
     *
     * 例如: API 过时, 不当使用 API, 不好的行为但不一定是错误的
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * 正常但需要突出的事件
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * 值得注意的事件
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * 详细的调试信息
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * 表示任意的等级的日志
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * logger-aware 实例
 */
interface LoggerAwareInterface
{
    /**
     * 设置一个日志记录实例在这个对象
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * 日志等级
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```
