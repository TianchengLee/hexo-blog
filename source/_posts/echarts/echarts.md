---
title: echarts使用
date:  2019-07-10 10:42:04
tags:
---

### echarts使用

- 1.grid 网格位置

             grid: {
                    top: 20,
                    left: 40,
                    right: 15,
                    bottom: 50,
                    containLabel: false,
                    height: 'auto'
                }

- 2.xAxis x轴 yAxis同样

         xAxis: [{
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'], //x轴显示
            nameGap:15,    //---坐标轴名称与轴线之间的距离(柱子与柱子之间的间距)
            axisLabel: {
                    interval:0, //设置成 0 强制显示所有标签（不写就间隔一个显示）
                    rotate:40, //x轴文字倾斜程度
                    show: true, //控制坐标轴x轴的文字是否显示
                　　textStyle: {
                　　color: '#758697', //x轴上的字体颜色
                　　fontSize:'16' // x轴字体大小
                    }
                }
            }]


            yAxis: [
                {
                    type: 'value',  //type=value时，y轴会根据最大data自动调整，例如data最大是7100，那y轴最大就是8000。
                    scale: true,
                    name: '价格',
                    max: 30,  //可以设置y轴最高数字
                    min: 0,    //y轴最小数字
                    boundaryGap: [0.1, 0.4],  //y轴每一行数字之间的间距大小 
                    nameTextStyle: {  //改变y轴标题title的字体颜色
                        color: 'red'
                    },
                    axisLine:{   //是否显示坐标轴线
                       show:false
                    },
                    axisTick:{  //是否显示坐标轴刻度 
                        show:false
                    },
                    splitLine:{  //分隔线样式设置
                        lineStyle:{
                            color:['#858895'],
                            type:'dashed'    //虚线 实现可选 'solid' 'dashed' 'dotted'
                        }
                 }
                }
            ],
            

- 3. series 内容部分

          series: [
                {
                    name: '销量',             //---系列名称
                    type: 'bar',                //---类型
                    legendHoverLink:true,       //---是否启用图例 hover 时的联动高亮
                    label:{                     //---图形上的文本标签
                        show:false,
                        position:'insideTop',   //---相对位置
                        rotate:0,               //---旋转角度
                        color:'#eee',
                    },
                    smooth:true,   //设置曲线平滑
                    itemStyle:{                 //---图形形状
                        normal: {     
                            color: function (params) {   //柱状图  各个柱子设置不同的颜色
                                var colorList = ['#5B9BD5', '#ED7D31', '#808000',];
                                return colorList[params.dataIndex];
                            },
                            label: {
                                show: true,
                                position: 'top',
                                formatter: '{c}'
                            }   
                        }
                    },
                    barWidth:'20',              //---柱形宽度
                    barCategoryGap:'20%',       //---柱形间距   柱状图取消间距，在series: 中设置属性barCategoryGap:-1即可
                    data: [3020, 4800, 3600, 6050, 4320, 6200,5050,7200,4521,6700,8000,5020]
                }
            ]

- 4.legend 图例

        legend: {
            orient: 'horizontal',   // 'horizontal' ¦ 'vertical'
            x: 'center',            // 'center' ¦ 'left' ¦ 'right'  图例位置改变
            y: 'top',              // 'top' ¦ 'bottom' ¦ 'center'
            left: "5%",            //或者使用left top right bottom
            bottom: "5%",
            backgroundColor: 'rgba(0,0,0,0)'，
            icon: 'circle',       //图例icon字样有'circle'椭圆，'rect'矩形，'roundRect'圆角矩形',triangle'三角形,'diamond'菱形,'pin'正圆, 'arrow' 箭头
            borderColor: '#ccc',       // 图例边框颜色
            borderWidth: 0,            // 图例边框线宽
            padding: 5,                // 图例内边距，单位px，默认各方向内边距为5
            itemGap: 10,               // 各个item之间的间隔，单位px，默认为10，/ 横向布局时为水平间隔，纵向布局时为纵向间隔
            itemWidth: 20,             // 图例 icon 宽度
            itemHeight: 14,            // 图例 icon 高度
            textStyle: {
                    fontSize: 22,         // 图例文字大小
                color: '#333'          // 图例文字颜色
            }
        }

-5. toolbox 工具栏

           toolbox: { //可视化的工具箱
                show: true,
                feature: {
                    dataView: { //数据视图
                        show: true
                    },
                    restore: { //重置
                        show: true
                    },
                    dataZoom: { //数据缩放视图
                        show: true
                    },
                    saveAsImage: {//保存图片
                        show: true
                    },
                    magicType: {//动态类型切换
                        type: ['bar', 'line']
                    }
                }
            },

-6. tooltip 弹框   

         tooltip: { //弹窗组件
            show: true,
            trigger: 'axis',  //只要有使用类目轴设置axis 如果是散点图，饼图 设置为item
            position: 'top',  //弹框显示位置 只有在设置trigger: 'axis'会生效
            axisPointer: {  //设置鼠标移动时候  线形的样式类型
                type: 'cross', // cross是十字架类型 shadow 阴影类型 line直线类型
                crossStyle: {  //对应的style设置
                    color: '#fff'
                },
                <!-- type: 'shadow', 
                shadowStyle:{
                     color: '#f7080'
                } -->
            }
            },


- echarts 树图 type='treemap'基础配置

        option = {
            color:['#FC6620','#3AA0FF','#698B22','#7FFF00'], //添加颜色 
            tooltip: {
                    formatter: function (info) {        //鼠标经过 浮框内容+样式
                        var value = info.value;
                        var name = info.name;     
                        return [name +':'+value,].join('');
                }
            },
            series: [{
                type: 'treemap',   
                breadcrumb:{                //false隐藏面包屑
                        show:false
                    },
                leafDepth:null,             //表示『展示几层』，层次更深的节点被隐藏起来。点击看到层次更深的节点  默认不开启为null undefinded
                label: {                    //设置显示字体内容  不设置默认只显示value
                    show: true,
                    formatter: '{b}：{c}'
                },
                data: [
                    {
                        "value": "1995",
                        "name": "鄂"
                    },
                    {
                        "value": "135",
                        "name": "豫"
                    },
                    {
                        "value": "125",
                        "name": "鲁"
                    },
                    {
                        "value": "115",
                        "name": "赣"
                    }
                    ]
                }]
        };


- 图例绑定事件

        myChart.on('click', function (value) {
                var type = value.name
                that.getXianChang(that.formatDate(new Date()), type, that.currentCity)
            });
            
        myChart.on('legendselectchanged', function(obj) {
            var selected = obj.selected;
            var legend = obj.name;

            // 使用 legendToggleSelect Action 会重新触发 legendselectchanged Event，导致本函数重复运行
            // 使得 无 selected 对象
            if (selected != undefined) {

                if (isFirstUnSelect(selected)) {
                    triggerAction('legendToggleSelect', selected);
                } else if (isAllUnSelected(selected)) {
                    triggerAction('legendSelect', selected);

                }
            }

        });

+  echarts：堆叠图
   
        series: [
            {
                name: '总量',
                type: 'line',
                smooth: true,    //设置折线平滑显示
                symbol: "none", //去掉折线的小圆圈点
                symbolSize: 5, //设置折线点小圆圈的大小
                stack: '总量',
                itemStyle: {
                    normal: { //颜色渐变函数 前四个参数分别表示四个位置依次为左、下、右、上
                        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                            offset: 0,
                            color: '#CB8993' // 0% 处的颜色
                        }, {
                            offset: 0.5,
                            color: '#CB8993' // 100% 处的颜色
                        }, {
                            offset: 1,
                            color: '#CC56CB' // 100% 处的颜色
                        }]), //背景渐变色
                        lineStyle: { // 系列级个性化折线样式
                            width: 0.5,
                            type: 'solid',
                            color: "#CC56CB"
                        }
                    }
                },
            
                areaStyle: {
                    normal: {}
                },
                data: [],
            }
        ]
