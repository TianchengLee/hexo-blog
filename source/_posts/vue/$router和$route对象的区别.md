---
title: $route $router对象的区别
date:  2019-07-16 10:42:04
tags:
---

-   路由传参，调用$router对象的push方法，向push方法传递一个路由配置对象

        this.$router.push({ path: "/AddAddress", query: { type: this.status} })
        this.$router.push({name:'AddAddress',params:{type:: this.status}})



-  query传递的参数会显示在url后面，如name=‘’张三“&age=”12“

        this.$route.params.type
        this.$route.query.type


-  $router 对象和$route对象两者区别：

-   1.$router 是VueRouter的一个对象，是通过Vue.use(VueRouter) 得到的一个router的实例对象，这个对象是全局的对象，包含所有路由的对象和属性

-   2.$route 是跳转的路由对象，每一个路由都会有一个route对象，是一个局部的对象