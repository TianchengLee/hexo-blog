---
title: echarts水晶球
date:  2019-07-30 10:42:04
tags:
---

##### echarts水晶球

-  1. 下载echarts-liquidfill.js

-  2.  <div id="jingli_id"></div>

-  3.   两个水晶球  data = [percent,percent]

                    // 1
                    var option1 = this.getDutyOption(data1);
                    option1.series[0].color = ["rgb(14,188,194)"];
                    option1.series[0].backgroundStyle.borderColor = "#6CFFFF";
                    this.setEchartOption("jingli_id",option1);
                    // 2
                    var option2 = this.getDutyOption(data2);
                    option2.series[0].color = ["rgb(228,159,68)"];
                    option2.series[0].backgroundStyle.borderColor = "#F5A623";
                    this.setEchartOption("daogang_id",option2);

                    getDutyOption:function(data){
                        var option = {
                            backgroundColor: "transparent",
                            series: [
                                {
                                    type: "liquidFill",
                                    radius: "80%",
                                    canter: ["50%","50%"],
                                    data: data,
                                    color: ["rgb(14,188,194)"],
                                    backgroundStyle: {
                                        borderWidth: 1,
                                        borderColor: "#6CFFFF",
                                        color: "rgb(43,55,81)" //水纹上方余留背景颜色
                                    },
                                    outline: {
                                        show: false //是否显示外圈线
                                        // borderDistance: 8,   //与外圈距离
                                        // itemStyle: {
                                        //     color: 'none',
                                        //     borderColor: '#294D99',  //线的颜色，粗细，模糊程度，模糊颜色
                                        //     borderWidth: 8,
                                        //     shadowBlur: 20,
                                        //     shadowColor: 'rgba(0, 0, 0, 0.25)'
                                        // }
                                    },
                                    label: {
                                        normal: {
                                            formatter: function(){
                                                return (data[0] * 100).toFixed(0) + "%"
                                            },
                                            textStyle: {
                                                fontSize: 22,
                                                color:'#fff'
                                            }
                                        }
                                    }
                                }
                            ]
                        };
                        return option;
                    },