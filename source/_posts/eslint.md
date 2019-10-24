---
title:  eslint关闭
date:  2019-02-23 16：55
tags:
---

## 关于eslint的问题

- 编写代码及浏览器预览时各种语法报错看着很伤，故修改了根目录下的配置文件 .eslintrc.js

- 修改 build 下 webpack.base.conf.js 中 rules 下第一个关于 eslint-loader 的规则注释掉即可，详见此 js 配置