<!--
author: zhengyu
date: 2015-11-20 16:15:02
title: php函数：__autoload、spl_autoload_register
tags: php,php函数
category: php
status: publish
summary: php函数：__autoload、spl_autoload_register
-->

函数作用
---

```spl_autoload_register```函数主要作用就是使用另外一个函数来实现实现```__autoload()```，说到这里就要解析一下```__autoload()```的用法，当php调用的class不存在时，```__autoload()```就会调用，然后你就可以在函数里面写处理语句，例如```require xxx.php```。

代码演示
---

C1.php:

```php
class C1{
	public function test(){
		echo "C1 class\n";
	}
}
```

C2_1.php:

```php
function __autoload($class){
	$file = $class . '.php';
	if (file_exists($file)){
		require_once($file);
	}
}

$c = new C1();
```

以上代码当运行C2_1.php时，由于找不到class C1，所以会调用```__autoload```方法，在方法里面```require_once```包含class C1的文件。

C2_2.php:

```php
function loader($class){
	$file = $class . '.php';
	if (file_exists($file)){
		require_once($file);
	}
}

spl_autoload_register('loader');

$c = new C1();
```

当你不想使用```__autoload```这个方法时，就可以使用```spl_autoload_register```，将函数名作为参数传入，既可将该函数当成```__autoload()```。

除了绑定函数，还可以绑定类静态方法或者类方法

绑定类静态方法：

C2_3.php:

```php
class Loader{
	public function __construct(){
		spl_autoload_register(array('Loader', 'autoloader'));
	}

	public static function autoloader(){
		$file = $class . '.php';
		if (file_exists($file)){
			require_once($file);
		}
	}
}

$l = new Loader();

$c = new C1();
```

绑定类方法:
```php
class Loader{
	public function __construct(){
		spl_autoload_register(array($this, 'autoloader'));
	}

	public function autoloader(){
		$file = $class . '.php';
		if (file_exists($file)){
			require_once($file);
		}
	}
}

$l = new Loader();

$c = new C1();
```
