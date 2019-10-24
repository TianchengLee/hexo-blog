---
title: elementui注意点
date:  2019-07-15 10:42:04
tags:
---



- 注意点:

    elementui 中的popover弹出层 想要修改每个div的背景色 需要在el-popover上 popper-class='xxxx'来添加类 才会生效

- 隐藏组件 滚动条(原生的滚动条会有一条竖向线条 这个是隐藏了滚动条 内容自动撑开)

  + 父级元素有没有设置固定高度

        <div class="search-item-box">
                <el-scrollbar>
                     xxx内容区域
                </el-scrollbar>
        </div>

            .scroll-item-bar{
                height: 680px;
            }
            .search-item-box .el-scrollbar__bar.is-vertical{
                width:2px;
                
            }
            .search-item-box .el-scrollbar__bar.is-vertical>div{
                background: #A1A1A1
            }

- dialog 弹出框设置动画效果

            @keyframes dialog-fade-in {
            0% {
            transform: translate3d(100%, 0, 0);
            opacity: 0;
            }
            100% {
            transform: translate3d(0, 0, 0);
            opacity: 1;
            }
        }
        
        @keyframes dialog-fade-out {
            0% {
            transform: translate3d(0, 0, 0);
            opacity: 1;
            }
            100% {
            transform: translate3d(100%, 0, 0);
            opacity: 0;
            }
        }

- el-table表格

    border 表示表格加上边框

    鼠标经过 显示详细信息 show-overflow-tooltip 显示
    
        <el-table-column
        prop="cjjg"
        label="采集机关"
        width="180"
        show-overflow-tooltip
        >
        <template slot-scope="scope">
            <div class="cell" :title="scope.row.cjjg">{{scope.row.cjjg}}</div>
        </template>
        </el-table-column>


  table 改变背景色 

        /deep/  .el-table::before,.el-table::after {left: 0;bottom: 0;  width: 100%;height: 0px;}  
        /deep/ .el-table{ 
            background: transparent !important;border:none!important;   //改变背景色
            th, tr{ 
                font-size:18px;background: transparent !important; color: #c0e7ff;text-align: center;
                }
            thead th, .el-table__expanded-cell { color: #4a90e2 !important;}
            td,th.is-leaf{ border-bottom: 1px solid rgba(0, 5, 140, 0.6) !important;}  //改变下边框颜色
            }
        /deep/ .el-table--enable-row-hover .el-table__body tr:hover > td {background: #2e6581;} //鼠标经过背景色
        /deep/ .el-table__body tr.current-row > td {background-color: rgba(4, 19, 147, 0.5);color: #c0e7ff;} //鼠标点击背景色

el-scrollBar 自定义样式

    .eventPrognosis-box ::-webkit-scrollbar {width:10px; height:5px; background-color:transparent;} /*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
    .eventPrognosis-box ::-webkit-scrollbar-track {background-color:#142B4F; border-radius:4px; } /*定义滚动条轨道 内阴影+圆角*/
    .eventPrognosis-box ::-webkit-scrollbar-thumb {background-color:#80BBCA; border-radius:6px;} /*定义滑块 内阴影+圆角*/
     /deep/ .el-scrollbar__bar{   
     &.is-horizontal, &.is-horizontal> div,&.is-vertical, &.is-vertical> div{ width: 0 !important;}  
      /deep/ .el-scrollbar__wrap {overflow: hidden;margin: 0!important;} //去除自带的横轴纵轴的滚动条
 } 