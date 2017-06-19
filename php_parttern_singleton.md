php设计模式-单例模式
======

### 问题：

* 需要一个对象可以被系统中的任何对象使用
* 对象不能被随意被覆盖
* 系统不能有超过一个该对象

### 实现：

```php
class Preferences {
    private $props = array();
    private static $insance;

    private function __construct() {}

    public static function getInstance()
    {
        if (empty(self::$instance)){
            self::$instance = new Preferences();
        }

        return self::$instance;
    }

    public function setProperty($key, $val)
    {
        $this->props[$key] = $val;
    }

    public function getProperty($key)
    {
        return $this->props[$key];
    }
}
```

由于构造函数是`private`的，所以并不能从外部实例化这个对象，然后通过getInstance方法生成一个唯一的对象并返回，这样用户就不能通过外部修改对象，而且由于getInstance方法是一个`static`方法，所以在脚本的任何地方都可以被调用。

### 调用示例：

```php
$pref = Preferences::getInstance();
$pref->setProperty('name', 'testName');

unset($pref); // 移除引用

$pref2 = Preferences::getInstance();
echo pref2->getProperty('name'); // 输出testName， 属性并没有因为unset而丢失
```
### 单例模式示意图：

![单例模式示意图](../img/单例模式示意图.png)

### 优缺点对比：

| 优点 | 缺点 |
| --- | --- |
| 作为全局变量的改进，让使用者不能使用错误的类型覆盖单例 | 和全局变量一样，可能会被误用 |
| - | 调试困难 |

> 以上内容来自对《深入php：面向对象、模式与实践》中“单例模式”的总结