---
title: js中开启全屏,退出全屏
date:  2019-07-11 15:42:04
tags:
---

-    开启全屏

        fullScreenChange: function () {
                var el = document.documentElement;
                var rfs = el.requestFullScreen || el.webkitRequestFullScreen || el.mozRequestFullScreen || el.msRequestFullScreen;//定义不同浏览器的全屏API
                //执行全屏
                if (typeof rfs != "undefined" && rfs) {
                    rfs.call(el);
                } else if (typeof window.ActiveXObject != "undefined") {
                    var wscript = new ActiveXObject("WScript.Shell");
                    if (wscript != null) {
                        wscript.SendKeys("{F11}");
                    }
                }
            },

- 退出全屏

        outFullScreenChange: function () {
            var el = document;
            var cfs = el.cancelFullScreen || el.webkitCancelFullScreen ||
                el.mozCancelFullScreen || el.exitFullScreen;
            if (cfs) { //typeof cfs != "undefined" && cfs
                cfs.call(el);
            } else if (typeof window.ActiveXObject != "undefined") {
                //for IE，这里和fullScreen相同，模拟按下F11键退出全屏
                var wscript = new ActiveXObject("WScript.Shell");
                if (wscript != null) {
                    wscript.SendKeys("{F11}");
                }
                }
            }, 