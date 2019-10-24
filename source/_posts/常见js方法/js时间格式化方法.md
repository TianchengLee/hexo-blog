---
title: js中格式化时间方法
date:  2019-07-11 15:42:04
tags:
---

- 1.replace替换

    '20170710'.replace(/(\d{4})(\d{2})(\d{2})/g,'$1-$2-$3');  // 2017-07-10


- 2.this.formatDate(new Date())  //格式化当日时间

      // 时间格式转换成HH:mm:ss
        formatDate: function (unixtimestamp) {
            var unixtimestamp = new Date(unixtimestamp);
            var year = 1900 + unixtimestamp.getYear();
            var month = "0" + (unixtimestamp.getMonth() + 1);
            var date = "0" + unixtimestamp.getDate();
            var hour = "0" + unixtimestamp.getHours();
            var minute = "0" + unixtimestamp.getMinutes();
            var second = "0" + unixtimestamp.getSeconds();
            var ymh = year + "-" + month.substring(month.length - 2, month.length) + "-" + date.substring(date.length - 2, date.length);
            var hms =
                hour.substring(hour.length - 2, hour.length) + ":" +
                minute.substring(minute.length - 2, minute.length) + ":" +
                second.substring(second.length - 2, second.length);
            // var total = ymh + " " + hms; //年月日 时分秒
            // return [ymh, hms, total]; 
            return ymh;
        }