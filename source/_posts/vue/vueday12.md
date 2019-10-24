---
title: 12 编程式的导航(使用js形式进行路由导航 this.$router.push)
date:  2019-01-08 18:35
tags:
---

## 编程式的导航

- 除了使用 <router-link> 创建 a 标签来定义导航链接，还可以借助 router 的实例方法，通过编写代码来实现

- 在 Vue 实例内部，通过 $router 访问路由实例。可以调用 this.$router.push

1.在网页中，有两种跳转方式：

   方式1： 使用 a 标签 的形式叫做 标签跳转 

   方式2： 使用 window.location.href 的形式，叫做 编程式导航 

2.使用JS的形式进行路由导航

   注意： 一定要区分 this.$route 和 this.$router 这两个对象，

   其中： this.$route 是路由【参数对象】，所有路由中的参数， params, query 都属于它

   其中： this.$router 是一个路由【导航对象】，用它 可以方便的 使用 JS 代码，实现路由的 前进、后退、 跳转到新的 URL 地址，这里面的this.$router指的是当前项目中的router实例

      console.log(this);

       1. 最简单的 // 字符串
      this.$router.push("/home/goodsinfo/" + id);

      2. 传递对象  // 对象 （如果使用path进行跳转，需要携带参数，必须自行手动拼接，path不能和params一起使用，如果传入了path 则params 无效）
      this.$router.push({ path: "/home/goodsinfo/" + id });

      3. 传递命名的路由  // 命名的路由（params 只能和name同时使用）
      this.$router.push({ name: "goodsinfo", params: { id } });

## 抽取组件要注意的事项

如果抽取组件之后，使用的时候，有部分组件比如宽高有冲突：

首页中的轮播图 和 详情中的轮播图，分歧点是 宽度到底是 100% 还是 自适应，

		<script>
			export default {
			  props: ["lunbotuList", "isfull"]
			};
		</script>
	
	 .full {width: 100%;}

	 <img :src="item.img" alt="" :class="{'full': isfull}">
	
