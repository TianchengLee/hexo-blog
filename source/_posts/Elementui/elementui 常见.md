---
title: elementui 常见css 及组件
date:  2019-10-24 10:42:04
tags:
---

- 注意：

        使用el-tabs 用el-row 包裹 <el-row> <el-col :span='24'><el-tabs>......</el-tabs></el-col> </el-row>

-  el-table样式：

 + 1.点击table 鼠标经过table样式：

        /deep/ .el-table--enable-row-hover .el-table__body tr:hover > td {
        background: rgba(4, 19, 147, 0.5);
        }
        /deep/ .el-table__body tr.current-row > td {
        background-color: rgba(4, 19, 147, 0.5);
        color: #c0e7ff;
        }

+ 2.table高度和默认样式

          .el-table {
                &::before {
                content: "";
                height: 0;
                }
                th.gutter {
                display: none;
                }
                th,
                td {
                    width: 100%;
                    height: 25px;
                    white-space: nowrap;
                    text-overflow: ellipsis;
                    overflow: hidden;
                }
                td,
                th.is-leaf {   //el-table设置border  每一行的边框样式修改
                border: 1px solid #061736 !important;
                }
                tr:nth-last-child {
                border-bottom: none !important;
                }
            }


+ 3. 特定行文字颜色 el-table-column加上class

        :class="tableRowClassName(scope.row)"


        el-button样式修改：
        .el-button:focus, .el-button:hover{}
        .el-button{}


+ 4.获取前七天时间

        getDataFuture: function() {
            let date2 = new Date(),
            time2=date2.getFullYear()+"-"+(date2.getMonth()+1)+"-"+date2.getDate()+' '+date2.getHours()+':'+date2.getMinutes()+':'+date2.getSeconds();//time1表示当前时间
            let date1 = new Date(date2);
            date1.setDate(date2.getDate()-7);
            let time1 = date1.getFullYear()+"-"+(date1.getMonth()+1)+"-"+date1.getDate()+' '+date1.getHours()+':'+date1.getMinutes()+':'+date1.getSeconds();
            this.dateForm =[time1,time2];
        },

+ 5.css 三角形

    border-width: 8px 10px 8px 0px;
    border-style: solid;
    border-color: transparent #fff transparent transparent;

+ 6.数组 find 找到符合条件的元素后返回 后面不再执行

          filter 返回符合条件的新的数组 
          map  var arr = [{a:1,b:2},{a:2,b:2},{a:3,b:3}] arr.map(e=>e.a)  //[1,2,3]
 	      var arr = [{a:1,b:2},{a:2,b:2},{a:3,b:3}] arr.filter (e=>e.a)  //[{a:1,b:2},{a:2,b:2},{a:3,b:3}]
	      var arr = [{a:1,b:2},{a:2,b:2},{a:3,b:3}] arr.find (e=>e.a)  //{a:1,b:2}

 + 7.下拉框样式：
        
        .el-select-dropdown__item.hover, .el-select-dropdown__item:hover{background-color: rgba(9,46,103,0.5);color:#fff;}
        .el-select-dropdown,.el-select-dropdown__item{
            background-color: #00689D;
            border: 1px solid #00689D;
            color: #fff;
        }

+ 8.解构赋值

        let data = {a: 1, b: 2}
        let {a, b} = data
        console.log(a, b) // 1 2

+ 9.join

    join 用分隔符来分隔数组中的元素

+ 10.select

  el-select 下拉 filterable 可输入搜索 clearable 清除查全部



+ 12.moment.js

        页面中引入  import moment from "moment";
        moment().format('YYYY-MM-DD'), // 日期选择器
        moment().hours() //转为小时
        moment().minutes() //转为分钟
        moment().seconds() //转为秒
        this.moment().add(1, 'months').format('YYYY-MM-DD');   //当前日期加一个月并输出格式为 'YYYY-MM-DD'


+ 13.elementui 上传

        <el-form-item label="车辆图片">
                  <el-upload
                    action="string"
                    class="upload-demo"
                    :http-request="uploadImg"
                    :limit="1"
                    :disabled="forbidden"
                    accept="image/png,image/jpeg"
                    :file-list="fileList"
                  >
                    <el-button
                      size="small"
                      :disabled="forbidden"
                      type="primary"
                    >点击上传</el-button>
                    <span
                      slot="tip"
                      style="color:#666;margin-left:6px;"
                      class="el-upload__tip"
                    >只能上传jpg/png文件</span>
                  </el-upload>
                </el-form-item>

        uploadImg(param) {
            let fileObj = param.file;
            //创建formdata对象，formData用来存储表单的数据，表单数据时以键值对形式存储的。
            let formData = new FormData();
            formData.append('file', fileObj);
            this.api({
                url: "",
                method: "post",
                data: formData
            }).then(res => {}) },
