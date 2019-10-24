---
title: 02 node.js __dirname  path模块
date: 2018-7-27 23:42:04
tags:
---
## node day02 ##

### node.js的简介 ###

- node.js是什么?

> 它不是一个新的语言, 也不是一个JS库, 它是一个内置了V8解析引擎的JavaScript运行时环境, 可以在node平台上直接运行js代码

- node.js的特点?

> node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效, 单线程, 异步

> 事件驱动: 如同前端开发时一样, 我们的开发都是伴随着用户的交互完成的, 最终目的是为了和用户实现交互, 例如: 当用户点击按钮时, 我们处理某些业务逻辑. node.js也一样, 一切的操作都是由事件来驱动的, 例如: 当有客户端来请求时我们做出对应的响应

> ​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             非阻塞式I/O: IO是指 input / output 输入输出, 一般泛指读写, 读写包括了文件读写, 网络读写. 非阻塞式是指, 如果有多个读写任务同时执行, 不会依次等待前面执行完后再执行后面, 而是将一个个读写任务放到任务队列中等待调度执行  拿票取餐

> 同理, 阻塞式IO就是指, 如果三个读写任务同时执行, 必须依次有顺序的进行, 第一个不读写完成, 无法继续读写第二个 嗨大米

> 单线程是指永远只有一条线程运行代码, 通俗的理解就是代码永远都由一个人负责执行

- 多线程对比单线程?

> 在大部分情况下(并发量较高), 多线程的性能肯定是优于单线程的

> 多线程的优势在于性能, 但是缺点在于数据同步问题, 如何解决? 加锁, 当一条线程在操作数据时, 其他现程必须等待, 执行完毕后才能继续下一条线程执行

- JS为什么是单线程?

> 因为js早期设计出来是应付前端开发, 操作DOM元素的, 如果使用多线程会出现一种情况: A线程说将DIV隐藏, B线程说将DIV显示, 最后结果不确定. 所以正因为多线程同时操作页面, 随机性太强了, 而且没有意义, 最后直接将js设计成了单线程

> 在Android开发中也是同理, 但是由于Android开发是Java语言进行的, 而Java是多线程语言, Android不得不做一个处理, 规定了一个主线程, 也被称为UI线程, 只能在该线程中操作UI, 如果在子线程操作UI 直接报错

### __dirname ###

在node中直接使用相对路径会出问题, 因为相对路径不是相对于当前文件所在路径, 而是相对于node指令执行的所在路径

	fs.readFile('./files/4.txt', (err, data) => {
	  if (err) return console.log(err.message)
	  console.log(data.toString())
	})

在控制台执行 `node __dirname文件读取.js`

可以正常读取到文件内容:

	C:\Users\LTC\Desktop\22期\课堂代码\nodeday01>node __dirname文件读取.js
	﻿aa
	ANSI是指操作系统平台的编码
	
	Windows默认是GBK
	
	Linux/Unix 默认是UTF-8

如果返回到上一级目录, 或者其他目录再去执行代码:

	C:\Users\LTC\Desktop\22期\课堂代码\nodeday01>cd ..

	C:\Users\LTC\Desktop\22期\课堂代码>node nodeday01\__dirname文件读取.js
	ENOENT: no such file or directory, open 'C:\Users\LTC\Desktop\22期\课堂代码\files\4.txt'

通过以上执行结果可以证明, 在node中执行相对路径是相对于`node`指令运行的目录

如何解决该问题?

我们希望相对于当前js文件所在的路径去写代码, 就需要利用`__dirname`全局成员完成绝对路径的拼接

	__dirname // 获取当前js文件所在的绝对路径

打印输出 `__dirname` 的值

	console.log(__dirname)

执行代码:

	C:\Users\LTC\Desktop\22期\课堂代码>node nodeday01\__dirname文件读取.js
	C:\Users\LTC\Desktop\22期\课堂代码\nodeday01

既然已经获取到绝对路径, 最后只需要进行字符串拼接即可完成路径操作:

	fs.readFile(__dirname + '/files/4.txt', (err, data) => {
	  if (err) return console.log(err.message)
	  console.log(data.toString())
	})

现在, 该代码在任何目录下执行都是指读取当前文件所在的目录下的files目录的4.txt

执行代码:

	C:\Users\LTC\Desktop\22期\课堂代码>node nodeday01\__dirname文件读取.js
	﻿aa
	ANSI是指操作系统平台的编码
	
	Windows默认是GBK
	
	Linux/Unix 默认是UTF-8

### __filename ###

不同于__dirname, 获取的是当前js文件的完整路径, 包括文件名, 所以不常用 了解即可

### fs.stat() ###

作用: 用于获取文件或目录的状态(属性)信息

用法:

	fs.stat(__dirname + '/files/4.txt', (err, stats) => {
 		stats // 表示获取到的数据信息对象
	})

结果stats中具有的属性如下:

	Stats {
	  dev: 2114,
	  ino: 48064969,
	  mode: 33188,
	  nlink: 1,
	  uid: 85,
	  gid: 100,
	  rdev: 0,
	  size: 527,
	  blksize: 4096,
	  blocks: 8,
	  atimeMs: 1318289051000.1,
	  mtimeMs: 1318289051000.1,
	  ctimeMs: 1318289051000.1,
	  birthtimeMs: 1318289051000.1,
	  atime: Mon, 10 Oct 2011 23:24:11 GMT,
	  mtime: Mon, 10 Oct 2011 23:24:11 GMT,
	  ctime: Mon, 10 Oct 2011 23:24:11 GMT,
	  birthtime: Mon, 10 Oct 2011 23:24:11 GMT }

还具备一些方法, 详见官方文档: [http://nodejs.cn/api/fs.html#fs_class_fs_stats](http://nodejs.cn/api/fs.html#fs_class_fs_stats "http://nodejs.cn/api/fs.html#fs_class_fs_stats")

### path模块 ###

唯一需要掌握的方法: **path.join()**

作用: 用于拼接路径片段, 例如: 

	path.join('c:\', 'xxx', './qqq', '../rrr')

	// c:\xxx\rrr

结合 `__dirname` 使用:

	path.join(__dirname, '../files') // 查找当前js文件所在目录的上一级目录中的files路径

其他方法:

	path.basename() // 文件名
	path.dirname() // 文件所在的路径
	path.extname() // 扩展名

### 模块化 ###

为了提高软件工程的后期可维护性, 代码的复用性, 降低查找Bug的难度

模块化该如何实现:

靠明文的规范, 例如前端模块化: AMD/CMD

node中模块化遵循的是 CommonJS 规范:

	require() 就是CommonJS规范中导入模块的方式

为什么浏览器中不使用CommonJS规范?

因为 CommonJS 是同步加载模块

### CommonJS规范 ###

CommonJS有以下3点规范:

1. 模块内部必须有require成员, 而且是一个函数, 用于导入其他模块
2. 模块内部必须有exports成员, 是一个对象, 用于向外暴露模块
3. 模块内部必须有module成员, 是一个对象, 表示当前模块

作用域:

每个js文件就是一个模块, 不同的模块之间作用域互不共享, 这种作用域被称为: **模块作用域**

在Node中也有全局作用域, 每个js文件都可以直接访问 global对象, 该对象就是全局对象, 但是开发中一般不会挂载全局成员, 为了避免 全局作用域的污染

模块之间暴露成员:

exports可以进行成员暴露, 但是不推荐, 因为exports只是node帮我们简化书写处理了一下, 是一个变量, 该变量记录了module.exports的值 类似于:`let exports = module.exports`

CommonJS暴露成员永远都以 `module.exports` 为主, 如果整个代码运行完毕, exports和module.exports指向同一个对象, 则不会出任何问题, 但是一旦当exports或module.exports修改了指向, 始终以module.exports为准, 所以总结如下:

**任何情况下都使用 module.exports**

### 包的规范 ###

1. 一个包必须放在一个目录下, 该目录必须由英文小写, 多个单词用`-`连接, 不能以数字开头

2. 包的根目录下必须有一个 `package.json`, 该文件中必须包含最少以下三个属性:

   	name, version, main

   但是不需要刻意的记忆以上三个属性, 在当前包的根目录下执行以下指令可以一键生成package.json文件:

   	npm init -y

