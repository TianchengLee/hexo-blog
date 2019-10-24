---
title: 散点 对应城市区域高亮
date:  2019-07-12 10:42:04
tags:
---

- 参照案例 https://www.cnblogs.com/amor17/p/9076150.html

- 遍历循环生成series 每个图标对应一个弹框tooltip

    for (let index in pointNumTotal) {
        var a = {
        type: 'scatter',    //类型为散点
        coordinateSystem: 'geo',  //使用的坐标系 'geo'表示地理坐标系 'cartesian2d'表示二维直角坐标系  'polar'表示使用极坐标系
        symbol: 'image://image/0.png',  //标记的图形 多种 点 箭头 圆等等 可以通过 'image://url' 设置为图片(url线上地址或者本地绝对路径)
        symbolSize: 50,   //标记的大小
        itemStyle: {
            normal: {
                color: '#F62157' // 标志颜色
            }
        },
         data: [{ name: xxxxx, value: [经度,纬度] }],  //data这个格式是因为拿的json文件是这样的,保持一致 才能正确获取到城市点位
         tooltip: {   //鼠标经过散点出现的弹框 自定义 
                position: 'top',
                formatter: function (params) {
                    return "<div style='width: 160px; height: 130px;'>" +
                        "<img src = './image/BGSS.png' style='width:100%;height:100%'>" +
                        "<div style='position: absolute;top:14px;left:22px'><p style = 'color : #7d3434'>" + pointNumTotal[index].name + "</p></div>" +
                        "<div style='position: absolute;top:56px;left:22px'>" +
                        "<p style = 'color : #7d3434;margin-bottom:5.1px'>入 : " + pointNumTotal[index].jcTotal + " 辆</p>" +
                        "<p style = 'color : #7d3434'>出 : " + pointNumTotal[index].ccTotal + " 辆</p>" +
                        "</div></div>";
                },
                backgroundColor: 'transparent'
            }
        }
        list1.push(a);
        }
        map1["series"] = list1;
        myChart.setOption(map1);


-  点击table  对应城市的区域高亮 以及展示tooltip弹出框  主要是设置regions属性

        var regions = [{
            name: city,
            itemStyle: {
                areaColor: '#8B658B',
                color: '#8B658B'
            }
        }];
        var map = {};
        var map1 = {};
        map["regions"] = regions;
        map1["geo"] = map;
        myChart.setOption(map1)



