---
title:  13 Vuex 与 getBoundingClientRect fetch方法
date:  2019-01-09 20：30
tags:
---
## Vuex

- 概念：
Vuex 是 Vue 配套的 公共数据管理工具，它可以把一些共享的数据，保存到 Vuex 中，方便 整个程序中的任何组件直接获取或修改我们的公共数据；

-  特点：
Vuex的状态存储式响应式的，（数据响应式,当Vue组件从仓库store 中读取数据的时候，store数据更新，对应的组数据也会更新）

##### 配置vuex的步骤
 
 1. 运行 	  cnpm   i   vuex -S 

 2. 导入包	import Vuex from 'vuex'
 
 3. 注册vuex到vue中  Vue.use(Vuex)

 4.  new Vuex.Store() 实例，得到一个 数据仓储对象
 
	     const store = new Vuex.Store({
		  state: { //表示仓库的数据
		    count: 0
		  },
		  mutations: { //用来操作仓库的数据
		    changeCount (state) {
		      state.count++
		    }
		  }，
		getters: { //获取仓库的数据,计算属性必须return
		  totalCount(state){
			return xxx
			}
		})

5. 将 vuex 创建的 store 挂载到 VM 实例上， 只要挂载到了 vm 上，任何组件都能使用 store 来存取数据

		  const  vm = new Vue({
		    el: '#app',
		    render: c => c(App),
		    store
		})

6. 要操作改变 state 中的数据，不推荐直接更改`this.$store.state.count` ，只要涉及到操作state的数据，要在 mutations 中定义一个方法，

  如果组件要调用mutations中的方法，通过提交一个任务给mutations进行执行：

	`this.$store.commit('定义的方法的名称'，传递的参数)`

	比如：  this.$store.commit('changeCount',组件要传递给store的参数名)

 1. mutations的 参数 最多两个，参数1：state，参数   2：commit 传递过来的数据

 2. getters  对数据进行包装。getters定义的函数可以直接当作数据使用。

  state中的数据变化，getters也会同步更新，类似于filter和computed的结合体

 3. 获取数据  
	
		 `{{ this.$store.getters.totalCount }}`
 
 4. 获取state中的数据  直接  'state.count'


##  getBoundingClientRect方法

getBoundingClientRect用于获取某个元素相对于视窗的位置集合。集合中有top, right, bottom, left等属性。

1.语法：这个方法没有参数。

rectObject = object.getBoundingClientRect();
2.返回值类型：TextRectangle对象，每个矩形具有四个整数性质（ 上， 右 ， 下，和左 ）表示的坐标的矩形，以像素为单位。

　rectObject.top：元素上边到视窗上边的距离;

　rectObject.right：元素右边到视窗左边的距离;

　rectObject.bottom：元素下边到视窗上边的距离;

　rectObject.left：元素左边到视窗左边的距离;

### fetch方法

	1. 基于xhrHttpRequest封装的一个联网请求 低版本IE不兼容 

	2.支持promise  使用时候需要自己封装 axios使用的更多