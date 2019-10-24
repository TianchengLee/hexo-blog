---
title: MUI使用过程遇到的问题
date:  2019-01-10 11:07:04
tags:
---
##  使用MUI遇到的问题

#### 一。制作顶部滑动条的坑们：

1. 需要借助于 MUI 中的 tab-top-webview-main.html 

2. 需要把 slider 区域的 mui-fullscreen 类去掉

3. 滑动条无法正常触发滑动，通过检查官方文档，发现这是JS组件，需要被初始化一下：
#
-   导入 mui.js 
 
-   调用官方提供的 方式 去初始化：

-   注意： 如果要初始化 滑动条，必须要等 DOM 元素加载完毕，所以，我们把 初始化 滑动条 的代码，搬到了 mounted 生命周期函数中；
	
		mounted（）{

			mui('.mui-scroll-wrapper').scroll({

		  	deceleration: 0.0005 //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006

		});
		}
		

4.我们在初始化 滑动条 的时候，导入的 mui.js ，但是，

控制台报错： Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode

	是因为 mui.js 中用到了 'caller', 'callee', and 'arguments' 东西，但是，
	 webpack 打包好的 bundle.js 中，默认是启用严格模式的，所以，这两者冲突了；

最终，我们选择了 plan B  移除严格模式： 使用这个插件

	 babel-plugin-transform-remove-strict-mode

或者在`.babelrc`文件中配置下忽略文件(与plugins并齐)：

		`"ignore": [
		"./src/lib/mui/js/mui.js"
		],`


####  二。mui.js引入之后，底部tab切换的时候产生冲突：

当 滑动条 调试OK后，发现，下面的 tabbar 点击切换组件的时候，无法正常切换，把引入进来的mui.js注释之后又恢复正常了，

是因为js和css文件里两者类样式名可能产生了冲突，需要把 每个 tabbar 按钮的 样式中  mui-tab-item 重新改一下名字，把里面的样式复制下来即可；

#### 三。使用MUI 