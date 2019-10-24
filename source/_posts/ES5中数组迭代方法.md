---
title: ES5中四种迭代数组的方法
date:  2018-12-27 23:42:04
tags:
---
title: ES5中四种迭代数组的方法
date: 2018-12-25 18:38:04
tags:

# ES5中四种迭代数组的方法
> forEach(): 单纯的循环  

> some(): 当回调函数返回true时就停止循环

> findIndex(): 找到指定条件的元素的索引

> filter(): 过滤

>  map() 高阶函数 生成新的数组 
 


## forEach()
- forEach循环数组的第一个参数为数组每一个的值，第二个为数组的索引，
  并且会循环每一项，数组有几项就会循环几次；

  	arr.forEach（（item,index）=> {
  		console.log('我在循环' + index)
  		if(条件表达式){
  		
  		}
  	}）

## some()
- some循环数组的第一个参数为数组每一个的值，第二个为数组的索引，可使用return true提前终止循环：

  	arr.some（（item,index）=> {
  		console.log('我在循环' + index)
  		if(条件表达式){
  			
  		return true
  		}
  	}）

## findIndex()
- findIndex循环数组，可使用return true终止遍历，同时返回该一项的索引

  	arr.findIndex（item => {
  		console.log('我在循环' + index)
  		if(条件表达式){
  			
  		return true
  		}
  	}）

## filter()
- filter循环数组，过滤元素，可使用return true返回该项的值，但不会终止遍历，遍历所有项之后会统一返回一个包含所有满足条件的项的数组：

  	arr.filter（item => {
  		console.log('我在循环' + index)
  		if(条件表达式){
  			
  		return true
  		}
  	}）

比如数组去重：

	var arr =[1,2,3,3,3,3,2,1];
      var newArr = [];
      arr.filter((item,i)=>{
          if(newArr.indexOf(item) == -1){
              newArr.push(item)      
              return true
          }
      })  
      console.log(newArr) //[1,2,3]

## map()
- map可以把数组所有数字转换成字符串,也可以把函数作为参数传递

		var arr = [1,2,3,4,5]
		arr.map(String)  => ['1','2','3','4','5']


		var f = function(x){
			return x*x
		}
		var str = [1,2,3,4,5]
		str.map(f)  => [1,4,9,16,25]

## reduce 函数累加器（ES5）
- 求和

		var sum = arr.reduce(function (prev, cur) {
			return prev + cur;
		},0);

- 求最大值

		var max = arr.reduce(function (prev, cur) {
			return Math.max(prev,cur);
		});