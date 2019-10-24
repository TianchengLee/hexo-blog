---
title: 16 vue-cli + element ui + axios + 路由的导航首位
date: 2018-1-17 23:42:04
tags:
---

# login-token-axios

> axios+token+Element UI

# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report


### 项目部署

1. 使用`vue-cli`脚手架搭建项目 
  	 
   准备环境： node（8以上），npm ，vue-cli（全局安装）

	vue init webpack ‘项目名称’

2. 安装包

    npm i less less-loader -D
	
    npm i element-ui axios vuex -S

3. 使用git/svn管理代码 这里使用的是git
  	
   在本地初始化仓库：
	
        git init 
 
 提交代码到本地仓库 

在github上建立空仓库，将本地仓库和github仓库建立关联，直接复制过来即可


###  使用Element-UI

问题1： 想要实现<el-menu>默认展开一个菜单项，在标签上添加
	:default-openeds="['1']"

就可以实现，这里‘1’ 是需要展开的子sub目录的index值。

问题2： 使用自带的一些属性值 要用 `：`绑定

1. 侧边栏部分使用了	nav	menu导航菜单

2. 收货地址部分使用了 	Table 表格


## axios的使用

1. 安装：

      npm install axios

2. 导入
     import axios from 'axios'

3. 配置根路径

   在main.js中配置根路径

    //配置根路径
	import axios from 'axios'
	axios.defaults.baseURL = 'http://litc.pro:9999/v1/'
	//在Vue的原型上挂载这么一个方法 axios 其他位置引用直接 使用 无需二次导入
	Vue.prototype.axios = axios

   在Vue的原型上挂载这么一个方法 axios 其他位置引用直接 使用 无需二次导入

	Vue.protoType.axios = axios

 4. 发送请求，get/post 发送请求时候要将token携带过去，作为身份识别：

		 getReceiverInfos() {
		       let token = localStorage.getItem("token");
		     axios
		        .get("users/getReceiverAddress",{headers:{Authorization:token}})
		        .then(result => {
		          this.receiverInfos = result.data.data;
		        })
		        .catch(err => {
		          console.dir(err);
		        });
		    },

5. 登陆成功之后，把获取到的token存储到本地localStorage中

5. 路由vue-router的导航守卫 作用:  在每一次路由跳转的时候, 都会触发一系列回调函数, 这些回调函数被称为导航守卫, 可以在这些回调函数中进行路由拦截操作

   	在进入某个路由之前进行拦截，必须调用next（）函数  将其引导到某个页面, 如果不传参数就是不干预路由跳转

		router.beforeEach((to, from, next) => {
		  // 在此处就需要判断, 是否能进入一些禁地(需要登录的页面)
		  // 如果添加了导航守卫的回调函数
		  let token = localStorage.getItem('token')

		//如果没有token或者当前页面不是login就return掉，不再执行，并且让其跳转到login页面，一定要注意此处要判断是不是已经在login页面，否则会成为死循环
		  if (!token && to.path !== '/login') {  
		    return 
			next('/login') 
		  }
		  next()
		})

6. axios的拦截器，有两个，请求拦截器，响应拦截器

    原理：根据axios发送出去的请求，都会经过拦截器的这道拦截操作，请求的最后一道关卡，配置好之后，后面再发送请求，不需要携带token，会自动去获取 本地的token 

		// 添加一个请求拦截器
		//此处的config就是get请求的第二个参数
		axios.interceptors.request.use(function (config) {
		  let token = localStorage.getItem('token')
			  if (token) {
			    config.headers.Authorization = token
			  }
		    return config;
		  }, function (error) {
		  
		    return Promise.reject(error);
		  });
		
		// 添加一个响应拦截器
		axios.interceptors.response.use(function (response) {
		  
		    return response;
		  }, function (error) {
		 
		    return Promise.reject(error);
		  });

## 拓展

  实现输入完用户名密码之后，回车进入登陆页面

 添加事件 @keydown.enter="login",发现没有效果，原因是：这里使用的第三方组件库，input是进行封装之后的，直接这样无法绑定事件

		@keydown.native.enter="login"

