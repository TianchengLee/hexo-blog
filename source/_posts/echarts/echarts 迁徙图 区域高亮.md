---
title: echarts 迁徙图
date:  2019-07-29 10:42:04
tags:
---



-  echarts实例 https://www.echartsjs.com/gallery/editor.html?c=geo-lines


-          			
				//1.拿到这样的数据结构 放射点
				var data = [
					[{name:'武汉'},{name:'上海',value:95}],
					[{name:'武汉'},{name:'北京',value:95}],
					[{name:'武汉'},{name:'长春',value:95}]
				]
				_this.xiyinData.forEach(function(item,index){
					_this.areaGroup.push(item);
					this.max = (this.maxone>this.maxtwo) ? this.maxone : this.maxtwo;
				});

					//2.迁徙图地区中心点
            		var geoCoordMap = {
						"江岸区":[114.3142,30.64701],
						"江汉区":[114.2592,30.6076],
						"硚口区":[114.21413,30.59723],
						"汉阳区":[114.21861,30.55396],
						"武昌区":[114.3371,30.54505],
						"青山区":[114.437497,30.6202],
						"经济技术开发区":[114.159446,30.487971],
						"洪山区":[114.418241,30.508135],
						"东西湖区":[114.085212,30.704136],
						"汉南区":[113.928814,30.309867],
						"蔡甸区":[113.966815,30.45807],
						"江夏区":[114.334918,30.261019],
						"黄陂区":[114.366678,30.880039],
						"新洲区":[114.801311,30.841286],
						"东湖高新区":[114.413148,30.548737],
						"东湖风景区":[114.413148,30.548737]
					};
					//3.鼠标经过的弹框 
        			var convertData = function (data) {
        				var res = [];
        				for(var i=0; i<data.length; i++){
        					var dataItem = data[i];
        					var fromCoord = geoCoordMap[dataItem[0].name];
        					var toCoord = geoCoordMap[dataItem[1].name];
        					if (fromCoord && toCoord) {
        						res.push({
        							fromName: dataItem[0].name,
        							toName: dataItem[1].name,
        							coords: [fromCoord, toCoord],
        							value: dataItem[1].value
        						});
        					}
        				};
        				return res;
        			};
        			// 4.放射性动画图标路径
        			var planePath = 'path://M1705.06,1318.313v-89.254l-319.9-221.799l0.073-208.063c0.521-84.662-26.629-121.796-63.961-121.491c-37.332-0.305-64.482,36.829-63.961,121.491l0.073,208.063l-319.9,221.799v89.254l330.343-157.288l12.238,241.308l-134.449,92.931l0.531,42.034l175.125-42.917l175.125,42.917l0.531-42.034l-134.449-92.931l12.238-241.308L1705.06,1318.313z';
					//5 迁徙路线颜色
					var colors = [
						'#73D152','#5BB93B','#409E1F','#1194F6','#4EA9ED',
						'#ECB85C','#CC8F24','#3C53AF','#223273','#725AA6',
						'#13C2C2','#3436C7','#DF3939'
					];

				 //6.迁徙图数据 
				this.areaGroup.forEach(function (item, i) {
						_this.series.push(
						{
							name: item[0],
							type: 'lines',
							zlevel: 2,  //图形的层级
							symbolSize: 10,//标记的大小
							effect: {  //线特效的配置
								show: true,
								period: 6,//特效动画的时间，单位为 s。
								trailLength: 0,//特效尾迹的长度
								symbolSize: 10,//特效标记的大小
								symbol: "arrow",//特效图形 
							},
							lineStyle: { //线条样式
								normal: {
									color: colors[i],
									width: 2,
									opacity: 0.9,
									curveness: 0.2
								}
							},
							data: convertData(item[1])
						},
						{
							name: item[0],
							type: 'effectScatter', //散点（气泡）图
							coordinateSystem: 'geo',
							zlevel: 2,
							rippleEffect: { //涟漪特效相关配置。
								brushType: 'stroke'//波纹的绘制方式'stroke' 和 'fill'
							},
							label: {//图形上的文本标签
								normal: {
									show: true,
									position: 'right',
									formatter: '{b}'
								}
							},
							symbolSize: function (val) {//特效图形的大小 默认为10
								// return val[2] / 8;  // 数值差异太大
								return 10;
							},
							itemStyle: {
								normal: {
									color: colors[i]
								}
							},
							data: item[1].map(function (dataItem) {
								return {
									name: dataItem[1].name,
									value: geoCoordMap[dataItem[1].name].concat([dataItem[1].value])
								};
							})
						});
					});
					
				 option = {
						tooltip : {
							trigger: 'item',
							formatter:function(params, ticket, callback){
								if(params.seriesType=="effectScatter") {
									return "线路："+params.data.name+""+params.data.value[2];
								}else if(params.seriesType=="lines"){
									return params.data.fromName+">"+params.data.toName+"<br />"+ "流量数据：" + params.data.value;
								}else{
									return params.name;
								}
							}
						},
						visualMap: {
							min: 0,
							max: this.max,
							itemWidth: 10,
							text:['流量数据',''],
							realtime: false,
							calculable: true,
							right: 20,
							inRange: {
								color: ['#5EFF00','#FFD200', '#FF5F00'],
							},
							textStyle:{
								color: "#9f9f9f",
								fontSize: 10
							}
						},
						legend: {
							orient: 'vertical',
							top: '50',
							left: '10',
							itemWidth: 12,
							itemHeight: 12,
							data: this.legendName,
							textStyle: {
								color: '#333',
								fontSize: 10
							},
							selectedMode: 'single',
							// selected: { "洪山区": true }
						},
						geo: {
							map: 'wuhan',
							roam: true,  //开启鼠标缩放和平移漫游
							label: {    //图形上的文本标签
								normal: {
									show: true,
									color: '#4173B6',
									fontSize: 10,
								},
								emphasis: { //高亮状态下的多边形和标签样式。
									show: true,
									color: '#000',
									fontSize: 12,
								}
							},
							itemStyle: { //地图区域的多边形 图形样式。
								// areaColor: '#323c48',
								normal: {
									areaColor: "#19AEDB",
									borderColor:'#fff',
									borderWidth: 2,
								},
								emphasis: {
									areaColor: '#48D5FF',
									borderColor:'#188A89',
									borderWidth: 2,
								}
							},
							emphasis: {
								itemStyle: {
									areaColor: 'purple'
								}
							},
							data: data,
							roam: true,
							bottom: 15,
							top: 15,
							selectedMode:'single', //是否支持多个选中 'single'表示单选，或者'multiple'表示多选
						},
						series: this.series
    				};

					