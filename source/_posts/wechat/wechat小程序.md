---
title:  微信小程序
date:  2019-01-12 16：55
tags:
---


## wechat 小程序

特点: 

- 单向数据流，通过数据绑定将逻辑层的数据，渲染到视图层，如果要从视图层到逻辑层，需要通过事件绑定

- data中的数据改变,不会同步到view层,需要借助 Page.prototype
.setData (Page的原型) `this.setData({ })`来修改数据，包含两层操作：1.改变数据源 2.刷新页面（通知框架，数据源改变，需要重新渲染页面）

- 数据的输出: 小胡子语法`{{ msg }}`
 用差值表达式来申明有类型的值 如`checked="{{ false }}"`

- 事件绑定:  用bindxxx

- 如点击事件 `bindtap='changeFlag'` 在js中声明事件函数

- 配置文件在json文件中写 

- **遵循的是CommonJS规范，导入模块是 `require()`, 导出模块一般都是`module.exports` 可以导出一个对象或者函数**

### 小程序中有如下几个生命周期:

在app.js中定义:

1. onLaunch: 当小程序初始化完成，初次打开程序时,会触发 （全局只触发一次,例如切换到后台再切换回来也不会触发）

2. onShow: 当小程序启动，或从后台进入前台显示，或者成为焦点时候都会触发 onShow(显示)

3. onHide: 当小程序从前台进入后台，会触发 onHide(隐藏)

4. onError: 当小程序发生脚本错误，或者 api 调用失败时，会触发  并带上错误信息(只能捕获到运行阶段的异常)

除了生命周期约定好的钩子函数,还可以定义其他成员,作为其他页面的共享

### 页面的生命周期:

**压栈时触发:**

1. onLoad: 页面加载触发,适合做数据初始化


2. onShow:页面前台展示时,触发

3. onReady:页面渲染完后触发(例如设置标题)

**弹栈时触发:**

4. onHide:页面隐藏

5. onUnload:页面卸载(页面返回,小程序内部就会销毁这个页面,此时会触发.用于在页面卸载之前保存页面状态)

### 条件渲染 wx:if 与 hidden 

`hidden="{{isChange}}"`用于频繁切换显示隐藏的元素时

`wx:if` 是直接创建销毁元素

条件判断： `wx:if  wx:else`,如：

		<view wx:if='{{hasMore}}'>加载更多</view>
		<view wx:else>已经没有更多数据了</view>


### block标签

- 该标签只是单纯的将结构标签包裹

- 用来控制需要动态的显示隐藏的元素标签

### wx:for 列表渲染

- `wx:for=”{{students}}” `便利循环data中的`students`数组

- 输出 `{{ item.xxx}}`  默认循环的每一项是`item`

- 为了将数据和元素的勾选状态绑定产生关联,也需要使用`wx:key= "id" `来实现绑定，或者使用 `*this`
如果不需要考虑前后顺序改变,可以不加key

### 事件

- 在组件中绑定一个事件处理函数。加上bindxxx 如 `bindtap=''`

- 只能绑定某个事件名（不能加括号），不能直接写事件表达式

- 可以通过`data-xx=''`或者`id=''` 来传参，标识具体操作的是哪一条数据

## WXSS 

- 尺寸单位 `rpx` 

- 将设备分成750个rpx的宽度，原理是：为不同设备做适配，由小程序去计算不同设备应该对应多少的物理像素。根据屏幕宽度进行自适应




### 发送请求
  
- 发送请求不存在跨域，请求的地址必须在管理后台配置：

- 添加白名单，域名必须备案，服务端必须采用HTTPS
	
	     wx.request({
	     url: '', 
	     data: {x: '',y: ''},
	     success(res) {
	        console.log(res.data)
	     }
	     })

- `wx.request()` 没有对 promise 进行封装，为了代码的简洁先进行封装，例如：

	     module.exports = (url,data)=>{
	        return new Promise((resolve,reject)=>{
	            wx.request({
	                url: `https://locally.uieee.com/${url}`, 
	                data,
	                success:resolve,
	                fail:reject
	            })
	        })
	        }

在需要发送http请求的模块引入这个文件，如

     let http = require('../../utils/http.js')    //相对路径    

       onLoad: function (options) {
	        http('slides').then(res=>{
	        this.setData({swiper:res.data})
        })
        }

- 传参  `?id=‘xxx’`  例如：`url='/pages/list/list?id={{item.id}}' `

## promise

promise 最大的价值就是在 `.then` 里面返回一个promise对象，后面可以继续 `.then`下去

## wxs（WeiXin Script）

-  wxs 一般都是当作过滤器来使用 

