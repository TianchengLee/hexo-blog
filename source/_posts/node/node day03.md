---
title: 03 node.js Node包  http模块 nodemon工具的使用 基本的express服务器用法
date: 2018-7-27 23:42:04
tags:
---
## node day03 ##

### npm ###

Node包管理器 (Node Package Manager)

	npm init // 初始化项目

安装全局包:

	npm install -g i5ting_toc

安装完成之后就可以直接使用了, 所有的全局包都存放在:

	C:\Users\用户名\AppData\Roaming\npm

卸载全局包:

	npm uninstall -g i5ting_toc

安装项目中使用的包(本地包):

	npm install jquery

卸载本地包:

	npm uninstall jquery

**注意: 安装本地包之前一定要确定, 当前目录必须有合法的package.json文件, 如果没有该文件, 则需要执行 `npm init -y` 进行初始化**

-D 参数: 将包安装到开发依赖, devDependencies节点下, 表示项目在开发阶段需要用到这些包, 上线后就不需要使用了

install 可以简写为 i 

	npm i webpack -D

**npm install** : 将package.json中的dependencies和devDependencies节点的所有包都安装到本地

	npm install

简写:

	npm i

--production 参数: 用于安装项目上线所用到的依赖包, 开发依赖不安装

	npm i --production


### http模块 ###

使用http模块可以用于创建服务器

1. 导入http模块

		const http = require('http')

2. 创建服务器对象

		const server = http.createServer()

3. 监听请求事件

		server.on('request', (req, res) => {
			// req 请求对象
			// res 响应对象
			res.end('hello world')
		})

4. 启动服务器

		server.listen('8080', '127.0.0.1', () => {
			// 启动成功的回调函数
			console.log('服务器运行成功! http://127.0.0.1:8080')
		})

### 乱码处理 ###

直接通过end()方法返回数据, 浏览器会用默认的码表来解析数据, 所以如果返回中文会导致乱码问题, 需要在响应头中指定数据的编码格式

	server.on('request', (req, res) => {
		// 在end方法执行之前设置响应头
		res.writeHeader(200, {
			Content-Type: 'text/plain;charset=utf-8'
		})

		// req 请求对象
		// res 响应对象
		res.end('hello world')
	})

在request事件监听中, req对象可以获取到用户请求的路径:

	req.url

通过该属性可以用于判断用户请求的路径, 决定返回具体的内容

### 服务端渲染页面 ###

1. 安装art-template

		npm install art-template

2. 导入art-template

		const template = require('art-template')

3. 当用户请求时, 使用template方法进行拼接模板, 最后通过res.end()方法响应拼接的HTML字符串给用户

		server.on('request', (req, res) => {
			if (req.url === '/') {
				let htmlStr = template(path.join(__dirname, './views/index.html'), {name: 'xxx', age: 18})
				res.end(htmlStr)
			}
		})

### nodemon工具的使用 ###

monitor: 监视器

作用: 当服务器代码修改后自动重启服务器

安装:

	npm install nodemon -g

使用:

以前都是使用node运行js文件, 现在使用nodemon运行即可

	nodemon 1.js

开启以后无需再次关闭, 如果修改了1.js文件的代码, 将会自动重启

### express ###

> 基于node的http模块进行的封装, 没有对原生方法进行覆盖, 而是扩展更方便更好用的api

> 好用!

基本的express服务器用法:

1. 安装express

		npm install express -S

2. 导入express

		const express = require('express')

3. 调用express()方法创建服务器对象

		const app = express()

4. 服务器对象可以通过 get/post 等等方法来监听客户端的不同方式的请求

		app.get('/', (req, res) => {
			// res.end() // 原生的http模块的响应方法
			res.send() // express封装的高级方法, 直接处理了乱码问题
		})

5. 调用listen()方法开启服务器

		app.listen(3000, () => {
			console.log('服务器启动成功! http://127.0.0.1:3000')
		})

### express的常用api ###

`res.send()`

1. 响应纯文本内容
2. 响应对象数据(最终会被转换为JSON字符串)
3. 响应二进制数据


`res.sendFile()`

1. 响应静态资源

	注意: 如果只传入一个参数, 必须传入绝对路径, 如果传入两个参数, 第一个参数就是相对于第二个参数的路径来识别的