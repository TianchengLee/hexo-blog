---
title:  call apply bind
date:  2019-05-30 15：30
tags:
---

#### call apply bind

- call apply 相同点 第一个参数是指定的对象 不同点 参数

- call 第一个参数是对象 后面的参数 就是传入该函数的值
  apply 第一个参数是对象 后面的参数 就是数组 传入该函数
  
            foo.call(obj, 1, 2, 3)
            foo.apply(obj, [1, 2, 3])
- bind 参数和call一样

            var obj = {name: 'obj'};
            obj.fn = function () {
                console.log(this)
            };
            var o = {name: 'o'};
            var a = obj.fn.bind(o)  
            a() //{name:'o'}
            ---------------------------
            var obj = {name: 'obj'};
            obj.fn = function () {
                console.log(this)
            };
            var o = {name: 'o'};
            obj.fn.call(o)   //  与bind的区别是，call直接调用函数

#### 正则相关

- 
          (?=a) 表示我们需要匹配某样东西的前面。 
          (?!a) 表示我们需要不匹配某样东西。 
          (?:a) 表示我们需要匹配某样东西本身。 
          (?<=a) 表示我们需要匹配某样东西的后面。 
          (?<!a) 表示我们需要不匹配某样东西，与(?!a)方向相反。
          
          console.log("我是中国人".replace(/我是(?=中国)/, "rr")) // 输出： 'rr中国人'，匹配的是中国前面的'我是'
          console.log("我是中国人".replace(/(?!中国)/, "rr")) // 输出：'rr我是中国人'
          console.log("我是中国人".replace(/(?:中国)/, "rr")) // 输出：'我是rr人'，匹配'中国'本身
          console.log("我是中国人".replace(/(?<=中国)人/, "rr")) // 输出：'我是中国rr'，匹配的是中国后面的'人'
          console.log("我是中国人".replace(/(?<!中国)/, "rr")) // 输出：'rr我是中国人'

-  常见验证
 
 1.手机号    `/^1[345678]\d{9}$/`
 
 2.身份证号  `/^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/`
 
 3.银行卡    `/^([1-9]{1})(\d{15}|\d{18})$/`
 
 4.邮箱     `/^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/`
 
 5.url    `/^((https?|ftp|file):\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`