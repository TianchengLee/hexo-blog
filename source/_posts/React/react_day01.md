---
title: React简介
date:  2019-01-18 14:50:04
tags:
---
### React简介

- libary（库）：小而精，只提供特定的API，只专精于某一项功能，如vuex（数据管理库）jquery（专注于操作DOM）axios（专注于数据的请求）

- Framework（框架）：大而全，面面俱到的，在使用过程中，提供了用户所需要的完整的解决方案，如三大框架vue，react，angular

- 组件化：基于UI界面对代码进行封装重用

- 模块化：基于业务逻辑对代码进行封装重用

### React概念

- DOM：浏览器为了表现页面结构，使用js对象对元素标签进行封装，并提供了操作DOM对象的API

- 虚拟DOM：框架里面的概念，框架里面为了表现DOM元素使用js对象来模拟DOM元素和嵌套关系

 1.本质：js的对象，模拟页面DOM的嵌套结构

 2.目的：实现页面的高校更新

 3.实现高效更新的过程：

   - 3.1 页面第一次渲染时候，内存中先生成对象A

 - 3.2 当页面发生变化了，再在内存中生成一个新的对象B

 - 3.3 将AB对象进行对比，找出两者有差异的部分，存储在内存中为对象C

- 3.4. 在页面再次渲染时，只渲染C，就实现了高效更新

- 对象差异化对比 Diff算法

#### Diff算法

 1. TreeDIff:  最顶层的对比，判断两个虚拟DOM树类型是否一样。

	如果不一样，就直接渲染新的虚拟Dom
	如果一样，再去实现ComponentDiff 
	（	这里的组件是指tree里面的属性名）

2. ComponentDiff ：如果虚拟Dom类型一样，再对比组件类型是否一样，如果不一样，直接记录当前的组件，接着对比下一个组件；如果一样，再实现ElementDiff

3. ElementDiff：（这里可以看作是属性值）如果组件类型一样，再对比里面的元素是否一样，如果不一样，就记录下来，最后渲染这个虚拟DOM

	// tree属性的对比，相当是TreeDIff
	// name属性名的对比，相当是ComponentDiff
	// 属性值的对比，相当是ElementDiff
	const obj1 = {
		tree:'A',
		name:'zs'
		}

	const obj2= {
		tree:'B',
		name:'ls'
		}

## webpack4

配置文件webpack.config.js中使用module.exports 暴露文件

	module.exports = {
	   mode:"development"
	}

#### webpack-dev-server

> 

#### html-webpack-plugin插件

2. 安装自动生成内存中的index页面的插件：

     npm i html-webpack-plugin -D

##  创建基本的webpack4.x项目

1. 运行`npm init -y` 快速初始化项目
2. 在项目根目录创建`src`源代码目录和`dist`产品目录
3. 在 src 目录下创建 `index.html`
4. 使用 cnpm 安装 webpack ，运行`cnpm i webpack webpack-cli -D`
   + 如何安装 `cnpm`: 全局运行 `npm i cnpm -g`
5. 注意：webpack 4.x 提供了 约定大于配置的概念；目的是为了尽量减少 配置文件的体积；
   + 默认约定了：
   + 打包的入口是`src` -> `index.js`
   + 打包的输出文件是`dist` -> `main.js`
   + 4.x 中 新增了 `mode` 选项(为必选项)，可选的值为：`development` 和 `production`;


### html-webpack-plugin插件 自动去导入内存中的index页面
		const path = require('path')

		const HtmlWebPackPlugin = require('html-webpack-plugin') // 导入 在内存中自动生成 index 页面的插件
	
	// 创建一个插件的实例对象

		const htmlPlugin = new HtmlWebPackPlugin({
		  template: path.join(__dirname, './src/index.html'), // 源文件
		  filename: 'index.html' // 生成的内存中首页的名称
		})

#### 使用React

1. 安装包 

		cnpm i react react-dom -S 安装包


2. 导入：必须大写

		import React from 'react'
		import ReactDOM from 'react-dom' 

3. 使用react提供的API创建虚拟DOM元素：

	 const myh1 = React.createElement('h1', { title: '啊，五环', id: 'myh1' }, '你比四环多一环')

3. 使用react提供的API渲染虚拟DOM元素：
		 
		 ReactDOM.render(<Hello username={obj.name} age={obj.age} />,document.getElementById('app'))
	 


