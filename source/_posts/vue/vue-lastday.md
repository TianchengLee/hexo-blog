---
title: 15 vue-router 导航守卫 axios Element-UI
date:  2019-01-11 23：30
tags:
---


## Vue最后一天 ##

- Vue基础:

	+ 插值表达式
	+ 指令
	+ 事件处理
	+ 组件
	+ 动画
	+ 过滤器
	+ watch
	+ computed
	...

- vue-router 路由

	+ 路由切换跳转前进后退（后退@click="$router.go(-1)"）,结合watch监视路由变化

	+ **导航守卫**

- vue-resource / **axios(拦截器, 模块化)**

	+ http库

- vuex

	+ 数据共享

- **JWT  使用Token做登录状态保持**

- **Element-UI   桌面端的UI组件库**

### 登录项目的设计目标 ###

- 掌握技能点:

	+ [x] 项目部署
	+ [x] 项目管理
	+ [x] Element-UI的使用
	+ [x] JWT进行登录状态保持
	+ [x] 路由导航守卫的使用
	+ [x] Axios的基础和高级用法
	

### 项目部署 ###

1. 使用`vue-cli`脚手架建立项目

	预先准备环境: node(8+) + npm + vue-cli(npm i vue-cli -g),这是vue-cli2的版本演示，最新版安装已经更改了

		vue init webpack login-demo

	注意: 如果需要开启eslint或e2e测试等, 自行选择, 这里都选了N, 只开启了vue-router, 并使用npm管理项目

	演示案例:

		PS C:\Users\LTC\Desktop> vue init webpack login-demo2

		? Project name login-demo2
		? Project description Login demo with JWT
		? Author TianchengLee <ltc6634284@gmail.com>
		? Vue build runtime
		? Install vue-router? Yes
		? Use ESLint to lint your code? No
		? Set up unit tests No
		? Setup e2e tests with Nightwatch? No
		? Should we run `npm install` for you after the project has been created? (recommended) npm
		
2. 安装less或sass

	由于脚手架默认配置好了less和sass, 但是没有安装对应的包, 可以根据需求自行选择安装

		npm i less less-loader sass-loader node-sass -D

3. 安装项目中额外要用的包

		npm i element-ui axios vuex -S

4. 使用git/svn来管理代码

	在本地初始化仓库

		git init

	提交代码到本地

		git add .
		git commit -m "Init Project"
	
	在github中建立好仓库, 将本地仓库和github仓库进行关联并提交本地的代码到远程

		git remote add origin git@github.com:TianchengLee/login-demo.git
		git push -u origin master
	
### 使用Element-UI ###


### Axios的使用 ###

> Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

- 特性

	+ 从浏览器中创建 XMLHttpRequests
	+ 从 node.js 创建 http 请求
	+ 支持 Promise API
	+ 拦截请求和响应
	+ 转换请求数据和响应数据
	+ 取消请求
	+ 自动转换 JSON 数据
	+ 客户端支持防御 XSRF