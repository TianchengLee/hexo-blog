---
title: 05 body-parser解析post请求提交的数据 
date: 2018-7-27 23:42:04
tags:
---
## node day05 ##

### body-parser解析post请求提交的数据 ###

1. 安装body-parser中间件

		npm install body-parser

2. 导入body-parser

		const bodyParser = require('body-parser')

3. 注册中间件 (一定要在处理路由规则之前)

		app.use(bodyParser.urlencoded({ extended: false }))