---
title: yarn安装
date:  2019-01-10 11:07:04
tags:
---

- 1.全局安装yarn

    npm i yarn -g

-  2.新增环境变量

    - 在 sysdm.cpl 控制面板--系统属性--高级--环境变量--系统变量--Path双击--新键，

    - 把cmder所在目录 路径 复制过去 （文件名路径不能含中文）

    - 确定保存之后 直接通过右键 Cmder here 执行命令 

    -  yarn 命令

    - 比如 yarn add sass-loader node-sass --dev


- 3.通过npm下载的全局包 查看:

     C:\Users\ASUS\AppData\Roaming\npm

    通过yarn下载的全局包:

     yarn global add http-server

- 4.yarn 下载的包存储在 C:\Users\ASUS\AppData\Local\bin中，同样去系统变量中配置一下


