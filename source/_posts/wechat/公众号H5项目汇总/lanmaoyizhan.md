---
title:  微信公众号_H5
date:  2019-05-19 16：55
tags:
---

### 微信公众号H5项目

技术栈： 

- vue + vuex + axios +  vue-router + vant UI + vw适配

- vue 实现vw适配 适配好后在项目中直接使用px 会自动转换 参考链接

         https://www.w3cplus.com/mobile/vw-layout-in-vue.html

        
遇到的问题 

#### 请求头

- axios的默认请求头是Content-Type: application/json

        会出现发两次请求的现象，因为首先会发起一个options预请求，判断接口是否能正常通讯 第二个请求是携带餐宿的真正请求

        解决：设置axios的默认请求头axios.defaults.headers['Content-Type'] = 'application/x-www-form-urlencoded';
        格式是{‘phone’:'15900000','password':'123123'}
        后台不认识这个格式需要转换下
        安装qs  把请求体传参的data 用qs.stringify({phone:'10000'})转换下

#### 移动端安卓 input 默认背景问题

-  input的背景 在安卓端默认是灰色的 ios上边框会有虚线 全局App.vue中加入了css

         input{
            border-radius:0; /*ios 去除底部虚线边框*/
            outline: none;
            background-color:transparent;
            -webkit-appearance:none;
            FILTER: alpha(opacity=0); /*androd 去掉白色背景*/
            }

#### input和button同时使用的时候 安卓端点击input输入框 按钮上浮到input上去了

- 解决方案1 通过js 监听屏幕可见区域的高度 动态设置button的显示隐藏

           mounted() {
                //监听窗口改变 避免input输入 按钮上浮
                 var originalHeight = document.documentElement.clientHeight || document.body.clientHeight;
                let that = this
                window.onresize = function temp() { //ios 软键盘弹起不会触发这个事件
                  var resizeHeight = document.documentElement.clientHeight || document.body.clientHeight;
                  if (resizeHeight - 0 < originalHeight - 0) {
                    that.btnShow = false; //或者position:static
                  } else {
                    that.btnShow = true; //或者position:fixed
                  }
                };
                },

    在mounted中 监听窗口的改变 动态改变按钮的隐藏显示 v-show=‘btnShow’

- 解决方案2  改变整体布局结构 使用flex布局 但是这样 安卓端 点击input输入框 按钮也会上浮一半

#### ios端  vant ui的省市区组件area 出现滑动卡顿 滑动不了的现象

- area组件 在安卓端 滑动都是正常 ios端 iphone 6p出现 只能点 不能上下滑动拖动的情况 

- 解决 

        .van-picker-column {
                position:relative;
                z-index:1;
            }
            .van-picker-column:before {
                width:100%;
                height:100%;
                position:absolute;
                top:0;
                left:0;
                content:'';
                }
            .van-picker-column>ul {
                z-index:-1;
                position:relative;
        }

- 在vant的github issue上看到有人使用这段css代码解决了问题 亲测有效 可能是ul的高度被覆盖了

#### 上传图片 使用的是input type=file 部分华为手机 点选不了相册的照片

- （还未证实） 看到有人是这么处理的

         input 选择图片在安卓失效 解决方案

         https://blog.csdn.net/weixin_33811539/article/details/87508393

    原来的input输入框选择图片是这么写的

        <input type="file" multiple   accept="image/gif, image/jpeg, image/png" id="upload">

    但是在安卓手机上不能选择图片，所以改为如下：

        <input type="file" accept="image/*" capture="camera" multiple  id="upload" >

    但是这样的话，在ios手机上就只能拍照，所以在js中还要加入一下代码

        var u = navigator.userAgent;
        var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
        if(isiOS){
            document.getElementById("upload").removeAttribute("capture","camera");
        }
#### lanmaoyizhan 项目汇总

- 关于vw移动端适配 参考 大漠老师的

      https://blog.csdn.net/weixin_42048805/article/details/81162540（非原作者）
      
- rem适配

        const PxToRem = () => {
          document.addEventListener(
            'DOMContentLoaded',
            function() {
              var html = document.documentElement
              var windowWidth = html.clientWidth
              if (windowWidth > 640) windowWidth = 640
              if (windowWidth < 320) windowWidth = 320
              html.style.fontSize = windowWidth / 7.5 + 'px'
            },
            false
          )
        }
        
        main.js 引入 
                     import { PxToRem } from './util/tools'
                     PxToRem()


####  小程序分享海报 微信开发者工具上正常 手机测试图像部分加载不出来 生成不出来

- 原因
      小程序项目中涉及到的所有域名都需要在公众平台配置好 包括下载图像的域名 
            

##### 小程序自定义组件--解决组件间的代码复用

- 在components文件下新建组件文件

     如商品展示组件
 
        shop-
            index.js -- Component({
                // 组件的属性列表
                  properties: {
                    item: {
                      type: Object,
                    }
                  },
            })
            
            index.wxml
            
            index.json -- {
                              "component": true,
                              "usingComponents": {}
                          }
                          
            index.wxss
 
 - 在需要使用到的組件json文件中 注册引入
 
        {
            "usingComponents": {
                "shop":"/components/shop/shop"
            },
            "navigationBarTitleText": "搜索"
        }  
        
 - 在需要使用的组件wxml文件 使用
 
        <shop wx:for="{{list}}" wx:key="{{index}}" item="{{item}}"></shop>   
        
 #### canvas 分享朋友圈海报 
 
 - 制作容器  
 
         <view class="canvas-box">
            <canvas canvas-id='share' class="shareCanvas" style='width:{{windowWidth}}px;height:{{windowHeight}}px;'></canvas>
         </view>   
         
 - js部分 如果海报需要呈现用户头像 先使用wx.downloadFile API下载头像到本地
 
         cancasImg() {
                let userName = app.globalData.nickName;
                let that = this, dataInfo = that.data,
                    canvasW = dataInfo.windowWidth, canvasH = dataInfo.windowHeight,
                    imgW = canvasW / 2 - 130;
                // 背景图
                let canvasImg = "../img/bg.png",
                    canvasImg1 = "../img/logo.png",
                    canvasImg2 = "../img/titlebg.png",
                    wxCode = that.data.wxCode,
                    // 头像
                    userImg = that.data.userImg;
                let text = '"我一直使用微小鸭，外卖包材工厂直供平台，',
                    text2 ='价格便宜，服务好，一件就送货上门！"',
                    text1 = '快试试你的手气吧！';
                let ctx = wx.createCanvasContext('share');
                // 绘制背景块
                ctx.setFillStyle('#fff');
                ctx.fillRect(0, 0, canvasW, canvasH);
                // 绘制背景图
                ctx.drawImage(canvasImg, 0, 0, canvasW, canvasH);
                // 绘制背景图1
                ctx.drawImage(canvasImg1, 0, 30, canvasW, 180);
                // 绘制背景图2
                ctx.drawImage(canvasImg2, (canvasW / 2 - 92), (canvasH / 2 + 70), 180, 40);
                // 绘制小程序码
                ctx.save();
                ctx.beginPath();
                ctx.arc((canvasW / 2), (canvasH / 2), 60, 0, 2 * Math.PI);
                ctx.fill();
                ctx.clip();
                ctx.drawImage(wxCode, (canvasW / 2 - 40), (canvasH / 2 - 40), 80, 80);
                ctx.restore();
                //绘制用户昵称
                ctx.setFontSize(12);
                ctx.setFillStyle = "#FFE0D2";
                ctx.fillText(userName, (canvasW / 2 - 100), (canvasH - 115));
                //绘制分享文案
                ctx.setFontSize(15);
                ctx.fillText(text1, (canvasW / 2 - 60), (canvasH / 2 + 98));
                ctx.setFillStyle = "#FFF";
        
                ctx.setFontSize(12);
                ctx.fillText(text, (imgW + 30), (canvasH - 90));
                ctx.setFillStyle = "#FFF";
                ctx.maxWidth = canvasW - 140;
        
                ctx.setFontSize(12);
                ctx.fillText(text2, (imgW + 30), (canvasH - 70));
                ctx.setFillStyle = "#FFF";
                ctx.maxWidth = canvasW - 140;
                // 绘制用户头像
                ctx.save();
                ctx.beginPath();
                ctx.arc((canvasW / 2 - 130), (canvasH - 110), 20, 0, 2 * Math.PI);
                ctx.fill();
                ctx.clip();
                ctx.drawImage(userImg, (canvasW / 2 - 150), (canvasH - 130), 40, 40);
                ctx.restore();
                ctx.draw(false, that.saveCanvas);
                wx.hideLoading()
                that.setData({
                    showContent: true
                })
                setTimeout(function () {
                    that.setData({
                        showFoot: true
                    })
                }, 3000)
            },
            saveCanvas() {
                wx.canvasToTempFilePath({
                    canvasId: 'share',
                    success: (res) => {
                        wx.saveImageToPhotosAlbum({
                            filePath: res.tempFilePath,
                            success: (res) => {
                                wx.hideLoading();
                                wx.showModal({
                                    content: '图片已保存到相册，赶紧晒一下吧~',
                                    showCancel: false,
                                    confirmText: '好的',
                                    confirmColor: '#333',
                                    success: function (res) {
                                        if (res.confirm) {
                                            console.log('用户点击确定');
                                        }
                                    }, fail: function (res) {
        
                                    }
                                })
                                console.log('成功保存到手机系统相册', res)
                            },
                            fail: (err) => {
                                wx.hideLoading();
                                console.log('保存到手机系统相册失败', err)
                            }
                        })
                    },
                    fail: (err) => {
                        wx.hideLoading();
                        console.log('保存到手机系统相册失败', err)
                    }
                })
            }
        
 #### 小程序中使用 async/await
 
 - 引入regenerator库 小程序不支持async/await ES7语法
 
            下载regenerator，并把regenerator-runtime并放到utils目录下
 
            app.js：    global.regeneratorRuntime = require('./utils/regenerator/runtime-module')
            其他页面js：  const { regeneratorRuntime } = global
            这样不需要每个页面都require，优化体积
        
                  
 #### 小程序的状态管理 mobx 
 
 - 参考
        `https://segmentfault.com/a/1190000010372573`
        
   1. 引入mobx utils目录下mobx文件放入mobx.js,observer.js
   
   2. 建立store文件夹 放入其他页面需要共用的功能部分 比如 购物车 登录状态 订单状态 城市定位 优惠券等(weixiaoya)
   
   3.    cart.js--  import {observer} from '../../utils/mobx/observer
                    const { regeneratorRuntime } = global;
                    let Cart = function () {
                        extendObservable(this, {
                            totalNumber: 0, //商品总个数
                            list: [],
                            //计算总金额
                            get total() {
                                return this.list.reduce((total, item) => {
                                    return total + item.goods.reduce((itemTotal, goods) => {
                                        if (!goods.selected) return itemTotal;
                                        return itemTotal + goods.price * goods.count;
                                    }, 0)
                                }, 0).toFixed(2)
                            }
                        })
                    }
                    Cart.prototype.fetchData = async function () {}
                    
   4. 需要用到的pages页面 js中调用store中的数据
           
          import { observer } from '../../utils/mobx/observer';
          Page(observer({
                props: {
                  cart: require('../../stores/Cart'),
                },
                data:{}
            })
       
       html中调用 props.cart.totalNumber 