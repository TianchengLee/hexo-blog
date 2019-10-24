---
title: 01 node基础  fs模块
date: 2018-7-27 23:42:04
tags:
---

## node day01 ##

### node的简介 ###

- 前后端分离:

	前端负责写页面, 和发送ajax请求获取数据, 通过模板引擎进行渲染
	
	后端负责写接口, 提供数据

- node环境的JavaScript

	不同于浏览器中js, 不存在DOM和BOM的概念

	以前所说的js的组成: ECMAScript + BOM + DOM, 其实是指在浏览器中JS的组成

		document.getElementById() // DOM中的方法
		var arr = []
		arr.sort() // ECMAScript中规范的方法

	共同点: 只要是js, 都会遵循ECMAScrit规范

	node环境的js组成: ECMAScript + 全局成员 + 内置模块 + 第三方模块

	npm: node package manager (node包管理器)

- 使用node运行js

	打开终端(Windows控制台) , cmd

### ES6新语法 ###

1. let关键字

	由于JS历史遗留问题, 声明变量一直使用var关键字, 特点就是有变量声明提升, 没有块级作用域

	let关键字的出现就是为了解决上述两个问题, 特点如下:

	a. let声明变量必须声明后才可以使用, 如果在声明之前使用变量会直接报错: xxx is not defined

	b. let声明变量不赋值, 默认值也是undefined

	c. let声明的变量有块级作用域, 如果在if/while/for等有大括号的代码中声明变量, 出了这个大括号就无法访问了

	d. let和const都无法重复声明变量, 一旦变量被声明就不可再次声明

2. const关键字

	不同于let的是, const用于声明常量, 一旦被声明必须立即赋值, 并且永远不可修改

	a. const声明的常量必须立即赋值

	b. 不可被再次修改

	c. 其他地方同let

3. 解构赋值

	作用: 将对象的属性提取出来存在变量中, 例如, 将userInfo对象的name和age属性提取出来放到name和age变量中存储:

		let userInfo = {
			name: '谢俊',
			age: 30,
			gender: '男',
			sayHi: function() {
				console.log('大家好我是' + this.name)
			}
		}
		// 以前的做法:
		// let name = userInfo.name
		// let userAge = userInfo.age
		// let sayHi = userInfo.sayHi
		// sayHi()
		// ES6解构赋值的做法:
		let { name, age: userAge, sayHi } = userInfo
		// 以上代码只是将用户信息的属性值提取出来存到变量中了, 如果修改变量name或userAge, 不会影响userInfo中的数据, 因为这是基本数据类型, 传递数据都是值传递

4. 箭头函数

	作用: 为了解决函数内部this指向的问题, 箭头函数的特点就是, 函数内部的this永远指向函数外部的this

	语法:

		// 小括号里面写形参列表
		// 大括号里面写函数体
		() => {}

	简便写法:

	a. 如果形参列表只有一个参数, 可以省略小括号, 注意: 如果一个参数都没有或者两个以上的参数必须加上小括号`()`

	b. 如果函数体中只有一行代码, 那么右侧的大括号`{}`可以省略, 并且还需要去掉return关键字, 默认会把这一行代码执行的结果返回出去

5. 添加对象属性或方法的简便写法

	作用: 把和对象属性同名的变量快速的添加到对象中

	以前的写法

		let name = '谢俊'
		let age = 30
		
		let obj = {
		  name: name,
		  age: age
		}

	ES6的写法:

		let name = '谢俊'
		let age = 30
		let fn = function () {
		  console.log('我是fn')
		}
		
		let obj = {
		  name,
		  age,
		  fn,
		  sayHi() {
		    
		  }
		}


### fs模块 ###

fs模块属于核心模块, 只要安装了node环境就会携带, 使用方法先导入该模块:

	const fs = require('fs')

1. 读取文件

		fs.readFile()

	参数1: 路径 path, 可以是相对路径

	参数2: 可选值 编码

	参数3: 读取完成的回调函数, 该回调函数中有两个参数, 第一个是错误信息err, 第二个是读取的数据data, 如果没有指定编码, data是Buffer类型的数据, 可以通过toString转为字符串

2. 写入文件

		fs.writeFile()

	参数1: 路径

	参数2: 要写入的数据, 类型可以是string或buffer

	参数3: 可选值 编码 默认utf-8

	参数4: 回调函数, 该回调函数只有一个参数错误信息err

	**注意: 如果要写入的文件不存在, 会创建文件后写入**

3. 追加文件

		fs.appendFile()

	参数1: 路径

	参数2: 要追加的数据, 类型可以是string或buffer

	参数3: 可选值 编码 默认utf-8

	参数4: 回调函数, 该回调函数只有一个参数错误信息err

	**注意: 如果要追加的文件不存在, 会创建文件后追加**


4. 复制文件

		fs.copyFile()

	参数1: 源路径

	参数2: 目标路径

	参数3: 可选值 flags 无视掉

	参数4: 回调函数, 该回调函数只有一个参数错误信息err