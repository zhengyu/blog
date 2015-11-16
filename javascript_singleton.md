<!--
author: zhengyu
date: 2015-11-16 15:05:02
title: javascript单例模式
tags: javascript,单例模式,设计模式
category: javascript,设计模式
status: publish
summary: javascript单例模式代码实现
-->

单例模式主要是为了确保一个类只有一个实例，具体实现方式是判断当前类是否已实例化，如果有则返回保存的对象，如果没有则创建一个新的实例。

javascript中有多种方法实现，其中一种实现代码如下：

```javascript
//此处通过闭包，将对象保存在Singleton变量中
var Singleton = (function (){
	var instance;
	function init(){
		return {
			t: function(){
				console.log('test');
			}
		};
	}

	return {
		getInstance: function(){
			if (!instance) {
				return init();
			}

			return instance;
		}
	};
})();

//调用方法
Singleton.getInstance().t();
```

单例模式应用场景：

1. 网站的计数器，确保计数同步

2. 应用程序日志，确保日志的写入是由一个实例完成，保证日志的完整性

3. 数据库连接类，减少用于数据库链接的资源

还有其他的应用，不过不难看出单例模式主要用于两点：操作共享资源、减少需要频繁操作的资源