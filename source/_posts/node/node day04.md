---
title: 04 express托管静态资源 结合ejs   art-template模板引擎 router模块的抽取
date: 2018-7-28 11:42:04
tags:
---
## 1node day04 ##

### express托管静态资源(重点) ###

1. 导入express

2. 创建app实例

3. 使用use注册静态资源中间件, 调用express.static()指定要托管的目录

4. 启动服务器

   	const express = require('express')

   	const app = express()

   	app.use(express.static('./views'))
   	// 虚拟目录
   	// app.use('/xxx', express.static('./views'))
   	
   	app.listen(3000, () => {
   	  console.log('http://127.0.0.1:3000');
   	});

### express结合ejs模板引擎渲染页面(重点) ###

1. 安装ejs包

   	npm i ejs

2. 使用 `app.set('view engine', 'ejs)` 设置默认使用的模板引擎

3. 使用 `app.set('views', './ejs_pages')` 设置模板存放路径( 非必要的设置, 模板默认存放在`views`目录 )

4. 在客户端请求时通过`res.render()`方法渲染模板页面并返回给客户端

   参数1: 模板文件名(不需要加后缀名, 自动根据设置的模板引擎查找对应的后缀名文件)

   参数2: 数据对象 

   	const express = require('express')
   	const app = express()
   	
   	app.set('view engine', 'ejs')
   	
   	// 没有设置views的路径, 证明 views目录就是模板引擎默认存放目录
   	// 自定义模板存放目录
   	app.set('views', './ejs_pages')
   	
   	app.get('/', (req, res) => {
   	  // res.render怎么知道index.ejs在哪里?
   	  // res.render怎么知道用什么模板引擎进行渲染?
   	  res.render('index', {name: '谢俊', age: 30, hobby: ['洗荤脚', '按荤摩', '摸摸唱']}) // 没有指定后缀名, 会根据当前设置的模板引擎来查找对应的后缀名文件
   	})
   	
   	app.listen(3000, () => {
   	  console.log('http://127.0.0.1:3000');
   	});

### express结合art-template完成渲染(了解) ###

使用步骤:

1. 需要安装两个包: art-template express-art-template

2. 由于express默认不支持art-template, 只能自定义模板引擎, 使用 `app.engine()` 来自定义模板引擎

   参数1: 模板引擎的名称

   参数2: 渲染方法的引用, require('express-art-template')

3. 使用`app.set('view engine', '模板引擎的名称')`设置默认的模板引擎

4. 使用 `app.set('views', './art_pages')` 设置模板存放路径( 非必要的设置, 模板默认存放在`views`目录 )

5. 在客户端请求时通过`res.render()`方法渲染模板页面并返回给客户端

   	const express = require('express')
   	const app = express()
   	
   	// 1. 使用 app.engine() 方法自定义模板引擎
   	app.engine('html', require('express-art-template'))
   	// 2. 使用 app.set('view engine', '指定模板引擎名称') 来配置项目中用到的模板引擎
   	app.set('view engine', 'html')
   	// 3. 配置模板页面的存放路径
   	app.set('views', './art_page')
   	
   	app.get('/', (req, res) => {
   	  res.render('index.html', { name: 'zs', age: 22, hobby: ['玩游戏', '唱歌'] })
   	})
   	
   	app.listen(3000, () => {
   	  console.log('server running at http://127.0.0.1:3000')
   	})

### router模块的抽取 (重点★★★★★) ###

路由就是用户请求URL, 服务器响应的对应关系

如果将所有的对应关系都放在主程序中, 那主程序会非常大, 不便于后期维护, 所以将主程序和路由对应关系拆分开, 使用CommonJS模块化规范进行抽取

主程序.js文件:

	const express = require('express')
	const app = express()
	const router = require('./3.router.js')
	
	// CommonJS规范  将路由模块导入到当前程序中
	// 安装路由模块
	app.use(router)
	
	app.listen(3000, () => {
	  console.log('http://127.0.0.1:3000');
	})

router.js文件:

	const express = require('express')

	// 创建路由对象
	const router = express.Router()
	
	// 当用户请求 / 路径时  做出对应的响应, 这条匹配规则就被称为路由
	// 后端路由
	router.get('/', (req, res) => {
	  res.sendFile('./views/home.html', {root: __dirname})
	})
	
	router.get('/movie', (req, res) => {
	  res.sendFile('./views/movie.html', {root: __dirname})
	})
	
	router.get('/about', (req, res) => {
	  res.sendFile('./views/about.html', {root: __dirname})
	})
	
	router.get('/home', (req, res) => {
	  res.sendFile('./views/home.html', {root: __dirname})
	})
	
	module.exports = router


### MySQL模块的使用(重点) ###

安装mysql包:

	npm install mysql

基本连接结构:

	// 1.导包
	const mysql = require('mysql');
	// 2.创建连接对象
	const connection = mysql.createConnection({
	  host: 'localhost', // 主机
	  user: 'root', // 数据库用户名
	  password: 'root', // 数据库密码
	  database: 'study_mysql' // 要连接的库
	});
	
	// connect:连接  连接数据库
	// connection.connect();
	
	// 3.执行sql语句 执行sql语句会隐式的连接数据库, 无需手动调用connection.connect()
	connection.query('select * from users where id = 1', (err, results, fields) => {
	  if (err) return console.log(err);
	  console.log(results[0].username);
	  // console.log(fields) // 表字段
	});
	
	// 结束
	// connection.end();

插入语句:

	conn.query('insert into users set ?', user, (err, results, fields) => {
	  if (err) return console.log(err);
	  console.log(results);
	  // console.log(fields) // 表字段
	});

修改语句:

	// const user = { id: 1, age: 48, gender: '男' }
	// conn.query('update users set ? where id = ?', [user, user.id], (err, results, fields) => {
	// conn.query('update users set ? where id = ' + user.id, user, (err, results, fields) => {
	// ES6的模板字符串 `` 反引号
	const age = 70
	const id = 1
	conn.query(`update users set age = ${age} where id = ${id}`, (err, results, fields) => {
	  if (err) return console.log(err);
	  console.log(results);
	  // console.log(fields) // 表字段
	});


删除语句:

	conn.query(`delete from users where id = ${id}`, (err, results, fields) => {
	  if (err) return console.log(err);
	  console.log(results);
	  // console.log(fields) // 表字段
	})

### 模块的查找规则 ###

导第三方包

1. 在当前目录查找 node_modules目录
2. 在node_modules目录中查找 mysql 目录
3. 在node_modules/mysql中找到 package.json 文件  看其中的main属性对应的文件
4. 去加载main属性对应的文件 如果没有main属性 就会按照用户模块的查找规则:

   index  -->  index.js -->  index.json  --> index.node

   	require('./3.router')

### express中获取参数 ###

1. query传参

   客户端通过

   	http://127.0.0.1:3000/user?name=zs&age=18

   接口请求的时候, express服务器可以通过`req.query`获取参数

   	app.get('/user', (req, res) => {
   		// query属性是express帮我们封装的属性
   		req.query // {name: 'zs', age: 18}
   	})

2. params传参

   客户端请求数据:

   	http://127.0.0.1:3000/user/1/ls

   服务器配置路由规则:
   	app.get('/post/:articleId')
   	app.get('/user/:id/:name', (req, res) => {
   		// params属性也是express帮我们封装的属性
   		req.params // {id: 1, name: 'ls'}
   	})

RestfulAPI设计规范

get和post的区别:

从数据安全性角度来看, get和post都一样, 无非只是get请求把数据放在URL中提交, 而post将数据放在请求体中提交而已, 如果真正想看数据的有心人, 一定可以看到你提交的数据

从幂等性角度来看, get更安全

get是幂等, post是不幂等

幂等: 通俗来讲, 就是当请求发送给服务器后, 不论发送多少次请求得到的结果都是相同的, 就被称为幂等, 如果多次请求发送得到的结果可能不同, 则不幂等

http基于TCP协议的

http又是无状态协议

而tcp是面向连接的协议

无状态和面向连接之间的关系?

tcp/ip详解 卷1234

《http权威指南》


三次握手 四次断开