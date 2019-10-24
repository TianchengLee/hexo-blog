---
title: 关于promise
date:  2019-02-12 23:42:04
tags:
---
## 关于promise

- ES6提供的promise作为异步操作的新的解决方案
	1.代码更有可读性
	2.可以更方便的处理多种异步操作组合问题
	3.promise 本身就是个构造函数
	4.解决了回调函数多层嵌套的问题,但是还是需要通过回调函数的方式获取异步操作的数据
- ES7  async$awiat可以简化代码书写
	1. async 是用来修饰函数，异步函数
	2. awiat  只使用于async修饰过的函数内部，用来修饰promise实例的，awiat后面所跟的代码必须能返回一个promise实例


## 封装promise 发送请求基本步骤

+ 1.自定义一个函数 
+ 2.在自定义函数内部创建一个promise实例并return返回，new Promise时必须传入一个函数，参数是resolve，reject
+ 3.在promise 实例的函数内部  执行真正的异步操作
  3.1 在真正的异步操作成功时调用resolve将成功的数据返回出去
  3.2   reject 是将失败的数据返回出去
+ 4.真正使用的时候调用函数获取数据
  **调用 fetch().then()   .then的前面返回的是promise对象**
+ 前面的.then中return的promise实例内部resolve或者reject出来的数据 可以在后面的.then中接收

		function fetch(url){
		return new Promise(function(resolve,reject){	
			$.ajax({
			url:‘url’,
			type:'get',
			success:function(data){
				resolve(data)
			}
			error:function(err){
				reject(err)
			}
			})
		})
		}
		fetch().then(function(res){
		console.log（res）
		}，function(err){
		console.log （err）
		})

链式编程的风格：有多个请求需要嵌套的时候：

		fetch('aa').then(function(){
			return fetch('bb')
		}).then(function(){
			return fetch('cc')
		})

如果是使用async,定义一个使用async修饰的函数，简化上步代码更加简洁：

	 async function getdata(){
	     //fetch().then() 拿数据 
		const res1 = await fetch('aa')
		const res1 = await fetch('bb')
		const res1 = await fetch('cc')
	}


## 封装promise 读取文件

	const fs = require('fs')
	const path = require('path')

     //1.定义一个函数
	function getContent(filename){
	 //2.在函数内部创建一个promise实例并返回
	return new Promise(function(resolve,reject){
	 //3.在promise实例内部执行真正的异步操作

	fs.rendFile(path.join(__dirname,filname),'utf-8',(err,data)=>{
	    if(!err){
		//3.1 异步操作成功调用resolve将数据返回
	      resolve(data)
	}else{
		//3.2 异步操作失败调用reject将数据返回
		reject(err)
	}
	})
	})
	}


 调用封装好的函数来读取文件

	getContent(‘路径’).then(function(data){
		//这里的data是promise实例内部hi用reaolve的数据返回
		console.log(data)
	},function(err){
	    console.log(err)
	}
	})