PSR-4 的实现示例
================================

以下示例列举了遵从 PSR-4 的代码:

闭包示例
---------------

```php
<?php
/**
 * 特定项目实现示例
 *
 * 用 SPL 注册自动加载函数之后，以下代码会试图自 /path/to/project/src/Baz/Qux.php 加载
 * \Foo\Bar\Baz\Qux class:
 *
 *      new \Foo\Bar\Baz\Qux;
 *      
 * @param string $class 完全限定类名称。
 * @return void
 */
spl_autoload_register(function ($class) {

    // 特定项目命名空间前缀
    $prefix = 'Foo\\Bar\\';

    // 命名空间前缀对应的基础目录
    $base_dir = __DIR__ . '/src/';

    // class 是否使用命名空间前缀？
    $len = strlen($prefix);
    if (strncmp($prefix, $class, $len) !== 0) {
        // 不使用，执行下一个注册的自动加载器
        return;
    }

    // 获取相对 class 名称
    $relative_class = substr($class, $len);

    // 替换命名空间前缀为基础目录，替换相对 class 名称中的命名空间分隔符为目录分隔符，附加
    // .php
    $file = $base_dir . str_replace('\\', '/', $relative_class) . '.php';

    // 如果文件存在，去 require 它
    if (file_exists($file)) {
        require $file;
    }
});
```

类示例
-------------

下面示例实现类处理多个命名空间:

```php
<?php
namespace Example;

/**
 * 实现单个命名空间前缀，允许包含多个基础目录可选功能的实现示例
 *
 * 给定 foo-bar 包的类文件在文件系统的以下路径 ...
 *
 *     /path/to/packages/foo-bar/
 *         src/
 *             Baz.php             # Foo\Bar\Baz
 *             Qux/
 *                 Quux.php        # Foo\Bar\Qux\Quux
 *         tests/
 *             BazTest.php         # Foo\Bar\BazTest
 *             Qux/
 *                 QuuxTest.php    # Foo\Bar\Qux\QuuxTest
 *
 * ... 添加路径到 \Foo\Bar\ 命名空间前缀的类文件，如下所示:
 *
 *      <?php
 *      // 实例化加载器
 *      $loader = new \Example\Psr4AutoloaderClass;
 *      
 *      // 注册自动加载器
 *      $loader->register();
 *      
 *      // 为命名空间前缀注册基础目录
 *      $loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/src');
 *      $loader->addNamespace('Foo\Bar', '/path/to/packages/foo-bar/tests');
 *
 * 下行将导致自动加载器试图从 /path/to/packages/foo-bar/src/Qux/Quux.php 加载
 * \Foo\Bar\Qux\Quux 类:
 *
 *      <?php
 *      new \Foo\Bar\Qux\Quux;
 *
 * 下行将导致自动加载器试图从 /path/to/packages/foo-bar/tests/Qux/QuuxTest.php 加载
 * \Foo\Bar\Qux\QuuxTest 类:
 *
 *      <?php
 *      new \Foo\Bar\Qux\QuuxTest;
 */
class Psr4AutoloaderClass
{
    /**
     * 一个关联数组，其中键是命名空间前缀，值是命名空间中作为类的基础目录的数组
     *
     * @var array
     */
    protected $prefixes = array();

    /**
     * 注册加载器到 SPL 自动加载器的堆栈中去
     *
     * @return void
     */
    public function register()
    {
        spl_autoload_register(array($this, 'loadClass'));
    }

    /**
     * 为命名空间前缀添加一个基础目录
     *
     * @param string $prefix 命名空间前缀
     * @param string $base_dir 命名空间中类文件的基础目录
     * @param bool $prepend 如果为真，则前置基础目录到堆，而不是追加它。这将决定它是被第一
     * 个找到还是最后一个
     * @return void
     */
    public function addNamespace($prefix, $base_dir, $prepend = false)
    {
        // 标准化命名空间前缀
        $prefix = trim($prefix, '\\') . '\\';

        // 带一个尾部分隔符标准化的目录
        $base_dir = rtrim($base_dir, DIRECTORY_SEPARATOR) . '/';

        // 初始化命名空间前缀数组
        if (isset($this->prefixes[$prefix]) === false) {
            $this->prefixes[$prefix] = array();
        }

        // 保存命名空间前缀的基础目录
        if ($prepend) {
            array_unshift($this->prefixes[$prefix], $base_dir);
        } else {
            array_push($this->prefixes[$prefix], $base_dir);
        }
    }

    /**
     * 加载给定类名的类文件
     *
     * @param string $class 完全限定类名
     * @return mixed 成功映射的文件名称，或失败时布尔值 false
     */
    public function loadClass($class)
    {
        // 当前命名空间前缀
        $prefix = $class;

        // 通过完全限定类名的命名空间名称倒推找到映射文件名称
        while (false !== $pos = strrpos($prefix, '\\')) {

            // 保存尾随命名空间分隔符的前缀
            $prefix = substr($class, 0, $pos + 1);

            // 余下的是相对类名称
            $relative_class = substr($class, $pos + 1);

            // 尝试加载前缀和对应的类映射的文件
            $mapped_file = $this->loadMappedFile($prefix, $relative_class);
            if ($mapped_file) {
                return $mapped_file;
            }

            // 为下次迭代 strrpos() 的删除尾部命名空间分隔符
            $prefix = rtrim($prefix, '\\');   
        }

        // 找不到任何映射文件
        return false;
    }

    /**
     * 加载一个命名空间前缀和相对类映射的文件
     *
     * @param string $prefix 命名空间前缀
     * @param string $relative_class 相对类名称
     * @return mixed 如果没有映射文件被加载为布尔值 false，否则为被加载映射文件的名称
     */
    protected function loadMappedFile($prefix, $relative_class)
    {
        // 是否有此命名空间前缀的任何基础目录
        if (isset($this->prefixes[$prefix]) === false) {
            return false;
        }

        // 通过基础目录查找这个命名空间前缀
        foreach ($this->prefixes[$prefix] as $base_dir) {

            // 替换命名空间前缀为基础目录，替换命名空间分隔符为目录分隔符，在相对类名称追加
            // .php
            $file = $base_dir
                  . str_replace('\\', '/', $relative_class)
                  . '.php';

            // 如果存在映射文件，则包含 (require) 它
            if ($this->requireFile($file)) {
                // 是，我们完成了
                return $file;
            }
        }

        // 找不然它
        return false;
    }

    /**
     * 如果文件存在，从文件系统中包含 (require) 它
     *
     * @param string $file 包含 (require) 的文件
     * @return bool 如果文件存在为 ture，否则为 false
     */
    protected function requireFile($file)
    {
        if (file_exists($file)) {
            require $file;
            return true;
        }
        return false;
    }
}
```

### 单元测试

以下示例是对上述的类加载器单元测试:

```php
<?php
namespace Example\Tests;

class MockPsr4AutoloaderClass extends Psr4AutoloaderClass
{
    protected $files = array();

    public function setFiles(array $files)
    {
        $this->files = $files;
    }

    protected function requireFile($file)
    {
        return in_array($file, $this->files);
    }
}

class Psr4AutoloaderClassTest extends \PHPUnit_Framework_TestCase
{
    protected $loader;

    protected function setUp()
    {
        $this->loader = new MockPsr4AutoloaderClass;

        $this->loader->setFiles(array(
            '/vendor/foo.bar/src/ClassName.php',
            '/vendor/foo.bar/src/DoomClassName.php',
            '/vendor/foo.bar/tests/ClassNameTest.php',
            '/vendor/foo.bardoom/src/ClassName.php',
            '/vendor/foo.bar.baz.dib/src/ClassName.php',
            '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php',
        ));

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar',
            '/vendor/foo.bar/tests'
        );

        $this->loader->addNamespace(
            'Foo\BarDoom',
            '/vendor/foo.bardoom/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib',
            '/vendor/foo.bar.baz.dib/src'
        );

        $this->loader->addNamespace(
            'Foo\Bar\Baz\Dib\Zim\Gir',
            '/vendor/foo.bar.baz.dib.zim.gir/src'
        );
    }

    public function testExistingFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\ClassName');
        $expect = '/vendor/foo.bar/src/ClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\Bar\ClassNameTest');
        $expect = '/vendor/foo.bar/tests/ClassNameTest.php';
        $this->assertSame($expect, $actual);
    }

    public function testMissingFile()
    {
        $actual = $this->loader->loadClass('No_Vendor\No_Package\NoClass');
        $this->assertFalse($actual);
    }

    public function testDeepFile()
    {
        $actual = $this->loader->loadClass('Foo\Bar\Baz\Dib\Zim\Gir\ClassName');
        $expect = '/vendor/foo.bar.baz.dib.zim.gir/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }

    public function testConfusion()
    {
        $actual = $this->loader->loadClass('Foo\Bar\DoomClassName');
        $expect = '/vendor/foo.bar/src/DoomClassName.php';
        $this->assertSame($expect, $actual);

        $actual = $this->loader->loadClass('Foo\BarDoom\ClassName');
        $expect = '/vendor/foo.bardoom/src/ClassName.php';
        $this->assertSame($expect, $actual);
    }
}
