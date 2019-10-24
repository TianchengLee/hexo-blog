---
title: 浏览器识别 elementui table导出excel echarts图片导出
date:  2019-08-09 15:42:04
tags:
---

####  浏览器类型识别

- html放入table

      <!-- 放置导出的table -->
        <div id="isTabledownloadId"></div>

- 识别浏览器类型

                getExplorer:function(){
                    var explorer = window.navigator.userAgent;
                    //ie
                    if (explorer.indexOf("MSIE") >= 0) {
                        return 'ie';
                    }
                    //firefox
                    else if (explorer.indexOf("Firefox") >= 0) {
                        return 'Firefox';
                    }
                    //Chrome
                    else if (explorer.indexOf("Chrome") >= 0) {
                        return 'Chrome';
                    }
                    //Opera
                    else if (explorer.indexOf("Opera") >= 0) {
                        return 'Opera';
                    }
                    //Safari
                    else if (explorer.indexOf("Safari") >= 0) {
                        return 'Safari';
                    };
                },

- 要导出的数据 this.widTableData -- 当前数据  this.widTableDatav2--对比的历史数据 （两个table）

        //表格导出
        doWinExcelDownLoad:function(){
           //1.要导出的json数据
            var jsonData = ecdo._myDeeplone(this.widTableData);
            this.widTableDatav2.forEach(function(item,index){
                Object.keys(item).forEach(function(key){
                    jsonData[index]["pre"+key] = item[key];
                });
            });
            // 2.列标题
            var str = '<tr><td>当前日期</td><td>当前指数</td><td>当前速度</td><td>对比日期</td><td>对比指数</td><td>对比速度</td></tr>';
            // 循环遍历，每行加入tr标签，每个单元格加td标签
            for(var i = 0 ; i < jsonData.length ; i++ ){
                str += '<tr>';
                for(var item in jsonData[i]){
                    // 增加\t为了不让表格显示科学计数法或者其他格式
                    str += "<td>"+ jsonData[i][item] +"\t</td>";   
                };
                str += '</tr>';
            };
            var table = '<table id="downLoadTableId">'+str+'</table>';

            var filename = "index&speed"; // 导出excel名称
            // 3.判断浏览器
            if(this.getExplorer() == 'ie' || this.getExplorer() == undefined){
                $("#isTabledownloadId").show().html(table);
                setTimeout(function(){
                    HtmlExportToExcelForIE(filename,"downLoadTableId");
                    $("#isTabledownloadId").hide();
                },1000);
            }else {
                HtmlExportToExcelForEntire()
            };


            // 4.非IE浏览器导出Excel
            function HtmlExportToExcelForEntire(filename){
                var url = 'data:application/vnd.ms-excel;base64,';
                // 下载的表格模板数据
                var template = '<html xmlns:o="urn:schemas-microsoft-com:office:office" \
                                    xmlns:x="urn:schemas-microsoft-com:office:excel" \
                                    xmlns="http://www.w3.org/TR/REC-html40"> \
                                    <head><!--[if gte mso 9]><xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet> \
                                        <x:Name>'+filename+'</x:Name> \
                                        <x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions></x:ExcelWorksheet> \
                                        </x:ExcelWorksheets></x:ExcelWorkbook></xml><![endif]--> \
                                    </head> \
                                    <body>'+ table +'</body> \
                                </html>';
                // 下载模板
                window.location.href = url + base64(template);
            };

            // 5.IE浏览器导出Excel
            function HtmlExportToExcelForIE(filename,tableid) {
                try{
                    var curTbl = document.getElementById(tableid);
                    var oXL;
                    try{
                        oXL = new ActiveXObject("Excel.Application"); // 创建AX对象excel
                    }catch(e){
                        alert("无法启动Excel!\n\n如果您确信您的电脑中已经安装了Excel，"+"那么请调整IE的安全级别。\n\n具体操作：\n\n"+"工具 → Internet选项 → 安全 → 自定义级别 → 对未标记为可安全执行脚本的ActiveX初始化并脚本运行 → 启用");
                        return false;
                    };
                    var oWB = oXL.Workbooks.Add();   // 获取workbook对象
                    var oSheet = oWB.ActiveSheet;    // 激活当前sheet
                    var sel = document.body.createTextRange();
                    sel.moveToElementText(curTbl);   // 把表格中的内容移到TextRange中
                    // 全选TextRange中内容
                    try{ sel.select();  }catch(e1){ e1.description };
                    sel.execCommand("Copy"); // 复制TextRange中内容
                    oSheet.Paste();          // 粘贴到活动的EXCEL中
                    oXL.Visible = true;      // 设置excel可见属性
                    // 文件名
                    var fname = oXL.Application.GetSaveAsFilename(filename+".xls", "Excel Spreadsheets (*.xls), *.xls");
                    oWB.SaveAs(fname);
                    oWB.Close();
                    oXL.Quit();
                }catch(e){
                    alert(e.description);
                };
            };

            //6.转码
            function base64 (s) { return window.btoa(unescape(encodeURIComponent(s))) };

            }

- echarts图片导出

    myChart.getConnectedDataURL()echarts的api   拿到返回的base64的url 

               doWinPngDownLoad:function(){
                    var id = "winDetailId", name = this.currentRowData.name + "-指数&速度曲线图";
                    var myChart = echarts.getInstanceByDom(document.getElementById(id));
                    var url = myChart.getConnectedDataURL({   
                        pixelRatio: 5,　　          // 导出的图片分辨率比率,默认是1
                        backgroundColor: '#fff',　　// 图表背景色
                        excludeComponents:[　　     // 保存图表时忽略的工具组件,默认忽略工具栏
                            'toolbox'
                        ],
                        type:'png'　　              // 图片类型支持png和jpeg
                    });
                    var $a = document.createElement('a');
                    var type = 'png';
                    $a.download = name + '.' + type;
                    $a.target = '_blank';
                    $a.href = url;
                    // Chrome and Firefox
                    if (typeof MouseEvent === 'function') {
                        var evt = new MouseEvent('click', {
                            view: window,
                            bubbles: true,
                            cancelable: false
                        });
                        $a.dispatchEvent(evt);  //触发事件
                    }
                    // IE
                    else {
                        var html = ''
                        '<body style="margin:0;">'
                        '![](' + url + ')'
                        '</body>';
                        var tab = window.open();
                        tab.document.write(html);
                    }
                },